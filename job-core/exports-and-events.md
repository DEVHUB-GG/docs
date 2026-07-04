---
description: >-
  The JobAPI table and events you use to build a job inside a Job Core copy —
  run control, objectives, phases, the per-run store, quests, party rules and
  lifecycle events, including the exact rules the core enforces.
---

# ⚙️ Developer API & Events

Job Core is a **copy-per-job template**: you copy the resource, give it a unique
`Config.JobId`, and build your job's gameplay **inside the copy** (in the
`jobFunctionality/` folder). Your gameplay talks to the core through one global table —
**`JobAPI`** — and a set of **events**. You never edit the core itself. The shipped
`jobFunctionality/` demo (a phased gather → smelt → craft job) is the reference
implementation of everything on this page — read it alongside this document.

A few things that apply everywhere:

* Every **`JobAPI`** function takes the player's server id (`source`) as its first argument.
* The **action** functions (`StartJob`, `AddObjectiveProgress`, …) exist **server-side only**.
  On the client you get `JobAPI.On(...)` plus the pure phase helpers
  (`GetPhaseObjectives` / `BuildObjectiveIndex`) — nothing else.
* "Objective" = a shared, party-pooled counter (`current` / `target`). Any crew member's
  progress counts toward it and is synced to everyone.
* "Run" / "session" = one active job per party. It ends on **finish** (paid), **cancel**
  (unpaid), **fail** (unpaid), or when the party fully dissolves.
* **XP is banked instantly, money is not.** Each unit of progress grants the contributor
  their XP immediately and it is never taken back. The money `reward` only accumulates in
  the shared **pool**, which is paid on Finish — and forfeited on Cancel / Fail.

{% hint style="info" %}
**Events are namespaced by your `Config.JobId`** so two job copies on one server never
collide. You don't build the names by hand — `JobAPI.On('server:onJobStart', fn)` adds the
prefix for you when listening, and the `Ev('...')` helper (available on both sides) builds
a prefixed name when you need to fire one yourself.
{% endhint %}

***

## <mark style="color:yellow;">**Building the run — Config.BuildObjectives**</mark>

The one **required** hook. The core calls it **once per run**, server-side, with the
**leader's** `source`, the moment the leader starts the job. Whatever you return is the
run's initial objective set:

```lua
-- ctx = {
--   size         = party size the run is locked to,
--   missionCount = suggested objective count (scaling row + the leader's
--                  progression missionSlots bonus),
--   scaling      = the locked scaling row { size, missions, payout, xp },
-- }
Config.BuildObjectives = function(source, ctx)
    return {
        { key = 'collect', label = 'Collect crates', target = 3, xp = 10, reward = 250 },
        -- ...more entries, or JobAPI.GetPhaseObjectives(Config.Job.phases, 1) for phase 1
    }
end
```

Each objective entry:

```lua
{
    key    = 'deliver_1',           -- unique id you use with AddObjectiveProgress
    label  = 'Deliver the package', -- shown in the mission HUD
    target = 3,                     -- units needed to complete (floored, minimum 1)
    xp     = 25,                    -- xp per unit to the contributor (optional)
    reward = 500,                   -- money per unit into the shared pool (optional)
    -- optional PHASE metadata — drives the HUD phase label + filtering:
    phase  = 1, phaseLabel = 'Gather', phaseTotal = 3,
}
```

* Returning an empty table (or anything that isn't a table) **aborts the start** — the
  leader gets `false, 'error_try_again'`.
* The core only carries what you see above. World data — props, coords, icons, blips —
  stays **your** job's data; keep it in your own config and look it up by `key`
  (see `JobAPI.BuildObjectiveIndex` below).
* `xp` / `reward` are **per unit** and scaled at credit time by the locked party-size
  multipliers plus the contributor's progression and boost bonuses.

***

## <mark style="color:yellow;">**JobAPI — run control**</mark>

```lua
JobAPI.StartJob(source)          -- ok, key — leader only; begin a run for the caller's party
JobAPI.CancelJob(source)         -- ok, key — leader only; end the run, pool forfeit
JobAPI.FinishJob(source)         -- ok, key — leader only; split the pool and pay the crew
JobAPI.FailJob(source, reason)   -- ok, key — DEV ONLY (not leader-gated); end the run unpaid
JobAPI.IsOnJob(source)           -- bool   — is this player an active member of a run?
JobAPI.GetScaling(source)        -- { partySize, objectiveCount } or nil
```

* **StartJob** — one run per party (`'job_already_active'` otherwise). Starting locks the
  scaling snapshot to the party size at that moment: mid-run joins/leaves **never** rescale
  the run. Party sizes above the highest `Config.Scaling.bySize` row fall back to the
  largest configured row.
* **GetScaling** — the locked snapshot: `partySize` the run was sized for,
  `objectiveCount` the base objective count handed to `Config.BuildObjectives`.
* All four lifecycle calls return `ok, messageKey` — the keys map to `Config.Lang`
  entries (`'job_leader_only'`, `'job_not_active'`, `'job_not_finished'`, …), so you can
  surface failures with `Core.Notify(src, _T(key), 4000, 'error')`.

### <mark style="color:yellow;">Finishing early — exactly what FinishJob does</mark>

`FinishJob` may be called **before every objective is complete**, and what happens is
controlled by `Config.Session.requireAllComplete`:

* **`false` (the default):** the finish is **accepted at any time**. The crew is paid the
  pool **as it stands** — a half-done run pays half a pool. Nothing is projected or topped
  up: `pool` is only ever what completed progress already deposited.
* **`true`:** the finish is **rejected** with `false, 'job_not_finished'` until every
  objective of the run is done. Use this when partial payouts would break your economy.

The payout itself, when accepted:

* Each member's cut is `floor(pool × their split % / 100)` — the split the party has **at
  finish time** (see the party section for how mid-run changes reshape it).
* Only members still **present in the run** are paid. Anyone who left or was kicked
  mid-run gets nothing — their contribution stays in the counters, but their share was
  already redistributed when they left.
* Each paid member also rolls their progression `itemChance` for a bonus item, and their
  lifetime `completedJobs` counter increments.
* `server:onJobFinish` fires once **per paid member** with their exact amount.

If you gate finishing yourself (e.g. "hand in at the NPC only after `onAllComplete`"),
listen for `server:onAllComplete` — the core also toasts the crew at that moment.

### <mark style="color:yellow;">Failing a run — JobAPI.FailJob</mark>

`FailJob(source, reason)` is for **your job's failure conditions** — a timer running out,
cargo destroyed, an alarm tripped. It ends the run for the whole crew with **no payout**
(XP already granted stays), fires `server:onJobFail(source, reason)` per present member,
and shows them `reason` (or the `job_failed` lang line when omitted).

Unlike Finish/Cancel it is **not leader-gated**: any active member's `source` identifies
the run. That makes it a **trusted, server-side-only** call — never wire it directly to a
client-controlled event.

***

## <mark style="color:yellow;">**JobAPI — objectives & phases**</mark>

Add objectives to a live run and push progress. `AddObjectiveProgress` is the main call you
make from gameplay — it credits the shared counter, grants the contributor XP, tops up the
payout pool, and syncs the whole crew.

```lua
JobAPI.AddObjectiveProgress(source, key, amount)  -- bool — add shared progress (xp + pool + sync)
JobAPI.SetObjectiveProgress(source, key, current) -- bool — set progress absolutely (no xp/pool)
JobAPI.AppendObjectives(source, objectives)       -- bool — append objectives to the live run
JobAPI.GetObjectives(source)                      -- array of { key, label, current, target, completed } or nil
JobAPI.GetPhase(source)                           -- { index, total, label, keys, complete } or nil
```

### <mark style="color:yellow;">The rules the core enforces (clamping)</mark>

These are guaranteed — build your gameplay on them instead of re-implementing them:

* **`AddObjectiveProgress`** floors `amount` and rejects anything `<= 0`. Progress is
  **clamped to `target`**: overshoot is dropped, and XP / pool money are credited **only
  for the clamped delta** — hammering a finished objective can't farm rewards. Calling it
  on an already-complete objective returns `true` and does nothing.
* It also returns `false` for anyone who is **not an active member of a run** (no session,
  or they left it) — you don't need your own "is on job" check just to credit progress.
* **`SetObjectiveProgress`** clamps to `0..target` and does **no** XP / pool /
  contribution accounting — it is a state-repair tool (load a checkpoint, admin fix),
  not a reward path.
* **Completion fires once.** `onObjectiveComplete` / `onPhaseComplete` fire the first time
  a counter (or phase) reaches its target and never again for that objective/phase — even
  if you later lower the value with `SetObjectiveProgress` and re-raise it.

### <mark style="color:yellow;">AppendObjectives is idempotent per key</mark>

`AppendObjectives(source, objectives)` silently **skips any entry whose `key` already
exists in the run**. That has two practical consequences:

* Appending a **static list** (fixed keys, e.g. a phase from your config) is **safe to
  repeat** — if two members' handlers both append phase 2, the second call is a no-op.
* Appending **dynamically generated** objectives (random keys, per-run ids) is **not**
  protected — every call adds new entries. Since `server:onJobStart` fires **once per
  member**, an unguarded dynamic append there runs N times for an N-member crew. Guard it:

```lua
JobAPI.On('server:onJobStart', function(source, partyId)
    if not JobAPI.IsLeader(source) then return end   -- run one-time setup exactly once
    JobAPI.AppendObjectives(source, buildRandomContracts())
end)
```

`target` is floored with a minimum of 1, exactly as in `Config.BuildObjectives`.

* **GetPhase(source)**: for phased jobs, the current phase — `index` / `total`, its `label`,
  the objective `keys` in it, and `complete` (all of the phase's objectives done). Returns
  `nil` when the job declares no phases **or the caller has no active run** — which makes it
  double as an "on job and in phase X" check. Job Core also fires **onPhaseComplete** the
  instant a phase's objectives are all done, so you usually react to that instead of polling.

### <mark style="color:yellow;">Phase helpers (optional)</mark>

If you build a **phased** job, these turn a plain phase config into the objects the core wants.
Both are **pure** — you pass your own `phases` array in (e.g. `Config.Job.phases`), an array of
`{ label, objectives = { { key, label, target, xp, reward, ... }, ... } }`:

```lua
JobAPI.GetPhaseObjectives(phases, index)  -- core-format objectives for one phase (nil if none)
JobAPI.BuildObjectiveIndex(phases)        -- flat  key -> definition  lookup across every phase
```

* **GetPhaseObjectives(phases, index)**: returns the phase's objectives tagged with phase
  metadata — feed straight into `AppendObjectives` (or return it from `Config.BuildObjectives`
  for phase 1).
* **BuildObjectiveIndex(phases)**: a `key -> definition` lookup so your gameplay can read a
  key's own data (prop / coords / label / etc.) without walking the phases each time. It is
  also your server-side **whitelist** for client-reported keys (see the security box below).

***

## <mark style="color:yellow;">**JobAPI — the per-run store**</mark>

Arbitrary job state scoped to the **active run** — routes, layouts, deadlines, spawned
entity ids. Use it instead of your own `[partyId]`-keyed tables:

```lua
JobAPI.SetRunData(source, key, value) -- bool — set (value = nil deletes) one entry
JobAPI.GetRunData(source, key)        -- the value, or nil without an active run
JobAPI.GetRunData(source)             -- the whole store table
```

* **It dies with the run — on every path.** Finish, cancel, fail, party disband, the whole
  crew leaving, an expired reconnect window: the store is wiped with the session. Your own
  `local State = {}` tables keep entries forever when a run ends abnormally; the store
  can't leak.
* **It survives reconnects.** A member who crashes and reclaims the run within the
  reconnect window comes back to the same store.
* Values are stored **by reference**, not copied — mutating the table `GetRunData` returns
  updates the run's state directly.
* Any active member's `source` reads the same store — it is per **run**, not per player.

The demo uses it for the phase-1 pickup layout: generated once per run, shared by the
whole crew, gone the moment the run ends however it ends:

```lua
local layout = JobAPI.GetRunData(src, 'demoLayout')
if not layout then
    layout = generateLayout()
    JobAPI.SetRunData(src, 'demoLayout', layout)
end
```

***

## <mark style="color:yellow;">**JobAPI — quests**</mark>

```lua
JobAPI.AddQuestProgress(source, chainId, amount) -- bool — advance a persistent quest chain
JobAPI.GetQuestProgress(source)                  -- array of quest chains with per-step state
```

* Quests are **persistent** (they survive runs, relogs and restarts) and separate from a
  run's mission objectives — drive them from any gameplay with `AddQuestProgress`.
  Completing a step (or the whole chain) grants its configured reward and fires the
  `Config.OnQuestStep` / `Config.OnQuestComplete` hooks.
* `GetQuestProgress` returns one entry per configured chain:
  `{ id, label, description, icon, percent, completed, currentStep, steps = { { id, label,
  target, progress, state = 'done'|'active'|'locked', ... }, ... } }`.

***

## <mark style="color:yellow;">**JobAPI — party / crew**</mark>

```lua
JobAPI.GetParty(source)         -- party payload or false
JobAPI.GetPartyMembers(source)  -- array of { source, identifier, name } or nil
JobAPI.IsLeader(source)         -- bool
JobAPI.GetLeader(source)        -- leader's server id or nil
JobAPI.SendToAllInParty(source, event, ...) -- fire a client event on every online member
```

* **GetParty(source)** returns `false` when the player has no party, otherwise:

  ```lua
  { id, youAreLeader, leaderIdentifier, maxSize, customSplit,
    members = { { source, identifier, name, level, isLeader, percent }, ... } }
  ```

* **SendToAllInParty(source, event, ...)**: triggers the given **client** event on every online
  member of the caller's party, forwarding any extra args. Solo = just the caller. Ideal for
  syncing your own gameplay state (e.g. "this prop was taken") to the whole crew:

  ```lua
  JobAPI.SendToAllInParty(src, 'myjob:client:removeProp', propId)
  ```

### <mark style="color:yellow;">Party changes mid-run — what happens to the run</mark>

A run belongs to the party that started it, but the party can keep changing underneath it.
These are the rules:

* **A member leaves / is kicked:** they are marked **absent** in the run — their client
  gets `client:jobEnded`, they can no longer contribute, and they are **not paid** at
  finish. The shared counters keep everything they contributed and the pool is untouched.
  The remaining members' split is re-evened, so their percentages **grow**.
* **The leader leaves or disconnects:** the next member is auto-promoted (the crew is
  notified). The **new** leader holds the run controls — Finish and Cancel gate on the
  party's *current* leader, not on whoever pressed Start.
* **Everyone leaves** → the run is dropped. **The leader disbands the party** → the run is
  torn down for the whole crew, unpaid.
* **Custom splits do not survive membership changes.** Any join, leave, kick or leader
  transfer resets the split to even (`customSplit = false`). A custom split must cover
  every member with integer percentages `0..100` summing to exactly `100`, or
  `setPayoutSplit` rejects it.
* **A disconnect is not a leave** (when `Config.Reconnect` is enabled): the dropped member
  can reclaim their spot within the window — solo runs suspend and wait, group runs keep
  going and readmit them if the party is still on the same phase and a slot is free.

{% hint style="warning" %}
**Inviting players mid-run costs the crew money.** The party will accept a new member
while a run is active, but the **run will not**: the joiner has no mission HUD, cannot
contribute, and is not paid at finish. Their even-split percentage still comes out of the
total, so that slice of the pool is paid to **no one**. Have the crew finish (or cancel)
the current run before growing the party.
{% endhint %}

***

## <mark style="color:yellow;">**Events — listening**</mark>

Listen to the core's events with **`JobAPI.On('<name>', fn)`** — it works for both server and
client events and adds the `Config.JobId` prefix for you.

```lua
-- server-side lifecycle
JobAPI.On('server:onJobStart', function(source, partyId) end)          -- per member
JobAPI.On('server:onJobCancel', function(source) end)                  -- per present member
JobAPI.On('server:onJobFinish', function(source, amountPaid) end)      -- per paid member
JobAPI.On('server:onJobFail', function(source, reason) end)            -- per present member
JobAPI.On('server:onObjectiveUpdate', function(source, key, current, target) end)
JobAPI.On('server:onObjectiveComplete', function(source, key) end)     -- once per objective
JobAPI.On('server:onPhaseComplete', function(source, phaseIndex, phase) end) -- once per phase
JobAPI.On('server:onAllComplete', function(partyId) end)               -- once per run
JobAPI.On('server:onLevelUp', function(source, newLevel, oldLevel) end)
```

**Per member or once per run?** This decides whether your handler needs a guard:

* `onJobStart`, `onJobCancel`, `onJobFinish`, `onJobFail` fire **once for every (present)
  member** — the leader included. One-time run work in these handlers needs an
  `if not JobAPI.IsLeader(source) then return end` guard (or must be idempotent, like a
  static `AppendObjectives`).
* `onObjectiveComplete` (per objective), `onPhaseComplete` (per phase) and
  `onAllComplete` (per run) fire **exactly once**. Their `source` is the member whose
  progress crossed the line — any crew member, not necessarily the leader.

Notes:

* **onJobStart** fires with the shared `partyId` — a good spot for one-time run setup
  (guarded, as above) and for seeding the per-run store.
* **onPhaseComplete** fires **before** the run's "all done" check, so appending the next
  phase's objectives from it keeps the run going; append nothing on the last phase and
  **onAllComplete** fires instead.
* **onJobFinish** fires per member with their exact `amountPaid` cut. Members may also
  receive a bonus item on finish — driven by their progression `itemChance` and granted
  automatically by Job Core.
* **onLevelUp** fires on any XP source that crosses a level — objective progress, quest
  rewards, tier-claim XP.

***

## <mark style="color:yellow;">**Events — the client side**</mark>

The core drives the in-world side of a run too. Listen with `JobAPI.On` on the client to spawn
props, blips and targets, and clean them up:

```lua
JobAPI.On('client:jobStarted', function(data) end)  -- a run started for you; data.vehicle = your resolved job vehicle
JobAPI.On('client:jobEnded',  function(data) end)   -- run ended (finish/cancel/fail/leave/kick)
JobAPI.On('client:syncSession', function(data) end) -- run state changed
```

* **jobStarted** fires when a run begins for you **and on reconnect** (see below). Use it for
  one-off setup or to request any state you need from your own server side. The core itself
  already handles the HUD and the job vehicle.
* **syncSession(data)** is the live picture of the run — reconcile your world to it on every fire:

  ```lua
  data = {
      active      = true,
      objectives  = { { key, label, current, target, completed }, ... },
      team        = { { identifier, name, contribution, present }, ... },
      phase       = { index, total, label, keys, complete }, -- nil if the job has no phases
      pool        = 12000,       -- shared payout pool so far
      yourShare   = 3000,        -- your projected cut at the current split
      allComplete = false,
      elapsed     = 145,         -- seconds since the run started
  }
  ```

* **jobEnded(data)** — tear down whatever you spawned. Fires on finish, cancel, fail, leaving
  the party, being kicked, or disconnect. It does **not** say which one — if your job cares,
  track that server-side via the lifecycle events.

{% hint style="info" %}
**Reconnect / anti-crash is built in.** If a player disconnects mid-run and returns within the
configured window, Job Core drops them straight back into the same run and re-fires `jobStarted`
+ `syncSession`. Because you already react to those events, your world simply respawns — and
whatever you kept in the per-run store is still there to answer their state requests. No
reconnect code needed on your side.
{% endhint %}

***

## <mark style="color:yellow;">**Your own gameplay events**</mark>

Everything above is the **core's** API. The events *you* invent for your job's own plumbing are
just normal FiveM events — `RegisterNetEvent` / `TriggerServerEvent` / `TriggerClientEvent`.
Build the names with `Ev('...')` so they inherit your `Config.JobId` prefix and stay unique
when the job is copied (the demo hardcodes a `jobcore_demo:` prefix instead — if you keep that
style, rename it in every copy):

```lua
-- client asks the server to work an objective
TriggerServerEvent(Ev('server:objUse'), key)

-- server validates, then credits the shared objective
local objectiveDefs = JobAPI.BuildObjectiveIndex(Config.Job.phases)
RegisterNetEvent(Ev('server:objUse'), function(key)
    local src = source
    if type(key) ~= 'string' or not objectiveDefs[key] then return end -- whitelist the key
    JobAPI.AddObjectiveProgress(src, key, 1)                           -- amount fixed server-side
end)
```

{% hint style="danger" %}
**Every networked event handler is attack surface.** A modified client can fire any net
event with any arguments, any number of times. When you write your job's server handlers:

* **Whitelist client input.** Check reported keys/ids against your own definitions
  (`BuildObjectiveIndex`, your layout in the run store) before acting on them.
* **Fix the numbers server-side.** Never take `amount`, `xp`, `reward` or a payout from
  the client — the client says *"I did the thing"*, the server decides what it's worth.
* **Re-check state, don't trust the premise.** The core already refuses progress from
  players not in the run, but everything else — distance to the prop, "was this pickup
  still available", cooldowns/rate limits for spammable actions — is yours to verify
  (the demo's pickup handler is the pattern: look the id up, check `taken`, then credit).
* **Keep trusted calls out of reach.** `JobAPI.FailJob`, `SetRunData`,
  `SetObjectiveProgress` and similar must only run from your own validated server logic —
  never as a 1:1 bridge from a client event.
* **Server listeners registered with `JobAPI.On` are net-reachable too** — a client can
  fire those names directly. Before doing something powerful in one (like appending a
  phase), re-verify the state with the getters, e.g. confirm `JobAPI.GetPhase(source)`
  really is complete.
{% endhint %}

***

## <mark style="color:yellow;">**Config hooks (the other half of the API)**</mark>

Every event above also has a plain-function twin in `configs/sh.main.lua` — handy when a
drop-in function beats wiring an event listener. All are optional no-ops by default:

```lua
Config.OnJobStart        = function(source, session) end
Config.OnJobCancel       = function(source) end
Config.OnJobFinish       = function(source, payout) end
Config.OnJobFail         = function(source, reason) end
Config.OnObjectiveUpdate = function(source, key, current, target) end
Config.OnObjectiveComplete = function(source, key, session) end
Config.OnPhaseComplete   = function(source, phaseIndex, phase) end
Config.OnAllComplete     = function(leaderSource, session) end
Config.OnTierUnlocked    = function(source, tier) end
Config.OnQuestStep       = function(source, chainId, stepId) end
Config.OnQuestComplete   = function(source, chainId) end
Config.OnShopPurchase    = function(source, cart, total) end
Config.OnShopSale        = function(source, cart, total) end
```

They fire at the same moments as their events (and with the same per-member / once-per-run
cadence). Unlike `JobAPI.On` listeners, hooks are **local-only** — they cannot be triggered
over the network.

***

## <mark style="color:yellow;">**Debug commands**</mark>

With `Config.Debug = true` the core registers test commands so you can exercise a job
without building the gameplay first: `/jobstart`, `/jobtest` (bumps the first incomplete
objective by 1), `/jobfinish`, `/jobcancel`, `/jobquest`, `/jobboost`, and `/jobui`
(opens the dashboard without the NPC).

***

## <mark style="color:yellow;">**Putting it together**</mark>

A compact server-side skeleton of an advanced job — phase 1 from the build hook, guarded
one-time setup, a hardened gameplay event, phase advancement, and a fail timer:

```lua
-- phase 1 comes from the REQUIRED build hook (called once, with the leader)
Config.BuildObjectives = function(source, ctx)
    return JobAPI.GetPhaseObjectives(Config.Job.phases, 1)
end

-- one-time run setup: onJobStart fires PER MEMBER, so guard dynamic work,
-- and keep per-run state in the store (auto-wiped however the run ends)
JobAPI.On('server:onJobStart', function(source, partyId)
    if not JobAPI.IsLeader(source) then return end
    JobAPI.SetRunData(source, 'route', pickRandomRoute())
    JobAPI.SetRunData(source, 'deadline', os.time() + 20 * 60)

    -- fail the run when the deadline passes (watchdog tied to the leader;
    -- re-arm it on leadership change if your leaders can leave mid-run)
    CreateThread(function()
        while JobAPI.IsOnJob(source) do
            local deadline = JobAPI.GetRunData(source, 'deadline')
            if deadline and os.time() > deadline then
                JobAPI.FailJob(source, 'You ran out of time!')
                return
            end
            Wait(5000)
        end
    end)
end)

-- your own gameplay event credits the objective — validated, fixed amount
local objectiveDefs = JobAPI.BuildObjectiveIndex(Config.Job.phases)
RegisterNetEvent(Ev('server:delivered'), function(key)
    local src = source
    if type(key) ~= 'string' or not objectiveDefs[key] then return end
    JobAPI.AddObjectiveProgress(src, key, 1)
end)

-- phased job: append the next phase the moment the current one is done.
-- onPhaseComplete fires once per phase; the state re-check keeps a spoofed
-- net call from skipping phases (JobAPI.On listeners are net-reachable).
JobAPI.On('server:onPhaseComplete', function(source, phaseIndex)
    local phase = JobAPI.GetPhase(source)
    if not phase or phase.index ~= phaseIndex or not phase.complete then return end
    local nextObjectives = JobAPI.GetPhaseObjectives(Config.Job.phases, phaseIndex + 1)
    if nextObjectives then JobAPI.AppendObjectives(source, nextObjectives) end
end)

JobAPI.On('server:onAllComplete', function(partyId)
    -- everything done — the crew heads back to the NPC and the leader Finishes
end)
```
