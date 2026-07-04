---
description: >-
  The JobAPI table and events you use to build a job inside a Job Core copy — run
  control, objectives, phases, quests, party and lifecycle events.
---

# ⚙️ Developer API & Events

Job Core is a **copy-per-job template**: you copy the resource, give it a unique
`Config.JobId`, and build your job's gameplay **inside the copy** (in the
`jobFunctionality/` folder). Your gameplay talks to the core through one global table —
**`JobAPI`** — and a set of **events**. You never edit the core itself.

A few things that apply everywhere:

* Every **`JobAPI`** function takes the player's server id (`source`) as its first argument.
* "Objective" = a shared, party-pooled counter (`current` / `target`). Any crew member's
  progress counts toward it and is synced to everyone.
* "Run" / "session" = one active job for a party. It ends on **finish**, **cancel**, or
  when the party dissolves.

{% hint style="info" %}
**Events are namespaced by your `Config.JobId`** so two job copies on one server never
collide. You don't build the names by hand — `JobAPI.On('server:onJobStart', fn)` adds the
prefix for you when listening. For your **own** gameplay events, use normal
`RegisterNetEvent` / `TriggerServerEvent` with your own names.
{% endhint %}

***

## <mark style="color:yellow;">**JobAPI — run control**</mark>

Start, stop and inspect a party's run. Leader-only calls return `ok, messageKey`.

```lua
JobAPI.StartJob(source)   -- ok, key  — leader only; begin a run for the caller's party
JobAPI.CancelJob(source)  -- ok, key  — leader only; end the run, no payout
JobAPI.FinishJob(source)  -- ok, key  — leader only; split the pool and pay the crew
JobAPI.IsOnJob(source)    -- bool     — is this player in an active run?
JobAPI.GetScaling(source) -- { partySize, objectiveCount } or nil
```

* **StartJob / CancelJob / FinishJob**: only the party **leader** may call these; a non-leader
  gets `false` plus a message key. Starting snapshots the party size for the whole run.
* **GetScaling**: the scaling locked in at start — `partySize` is the crew size the run was
  sized for, `objectiveCount` the base objective count.

***

## <mark style="color:yellow;">**JobAPI — objectives & phases**</mark>

Add objectives to a live run and push progress. `AddObjectiveProgress` is the main call you
make from gameplay — it credits the shared counter, grants the contributor XP, tops up the
payout pool, and syncs the whole crew.

```lua
JobAPI.AddObjectiveProgress(source, key, amount) -- bool — add shared progress (xp + pool + sync)
JobAPI.SetObjective(source, key, current)        -- bool — set progress absolutely (no xp/pool)
JobAPI.AppendObjectives(source, objectives)      -- bool — append objectives to the live run
JobAPI.GetObjectives(source)                     -- array of { key, label, current, target, completed } or nil
JobAPI.GetPhase(source)                          -- { index, total, label, keys, complete } or nil
```

* **AppendObjectives(source, objectives)**: `objectives` is an array. Each entry:

  ```lua
  {
      key    = 'deliver_1',     -- unique id you use with AddObjectiveProgress
      label  = 'Deliver the package', -- shown in the mission HUD
      target = 3,               -- units needed to complete
      xp     = 25,              -- xp per unit to the contributor (optional)
      reward = 500,             -- money per unit into the shared pool (optional)
      -- optional PHASE metadata — drives the HUD phase label + filtering:
      phase  = 1, phaseLabel = 'Gather', phaseTotal = 3,
  }
  ```

* **GetPhase(source)**: for phased jobs, the current phase — `index` / `total`, its `label`,
  the objective `keys` in it, and `complete` (all of the phase's objectives done). Returns
  `nil` when the job has no phases. Job Core also fires the **onPhaseComplete** event (below)
  the instant a phase's objectives are all done, so you usually react to that instead of polling.

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
  key's own data (prop / coords / label / etc.) without walking the phases each time.

***

## <mark style="color:yellow;">**JobAPI — quests**</mark>

```lua
JobAPI.AddQuestProgress(source, chainId, amount) -- bool — advance a persistent quest chain
JobAPI.GetQuestProgress(source)                  -- array of quest chains with per-step state
```

* Quests are **persistent** and separate from a run's mission objectives — drive them from any
  gameplay with `AddQuestProgress`. Completing a step (or the whole chain) grants its configured
  reward and fires the `Config.OnQuestStep` / `Config.OnQuestComplete` hooks.

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

***

## <mark style="color:yellow;">**Events — listening**</mark>

Listen to the core's events with **`JobAPI.On('<name>', fn)`** — it works for both server and
client events and adds the `Config.JobId` prefix for you.

```lua
-- server-side lifecycle
JobAPI.On('server:onJobStart', function(source, partyId) end)
JobAPI.On('server:onJobCancel', function(source) end)
JobAPI.On('server:onJobFinish', function(source, amountPaid) end)
JobAPI.On('server:onObjectiveUpdate', function(source, key, current, target) end)
JobAPI.On('server:onObjectiveComplete', function(source, key) end)
JobAPI.On('server:onPhaseComplete', function(source, phaseIndex, phase) end)
JobAPI.On('server:onAllComplete', function(partyId) end)
JobAPI.On('server:onLevelUp', function(source, newLevel, oldLevel) end)
```

* **onJobStart** fires once per member with the shared `partyId`. A great spot to inject your
  objectives with `AppendObjectives`.
* **onObjectiveComplete** fires the moment a shared objective is finished. **onPhaseComplete**
  fires once when every objective of the *current phase* is done (phased jobs only) — it's fired
  **before** the run's "all done" check, so appending the next phase's objectives from here keeps
  the run going. **onAllComplete** fires when every objective of the whole run is done.
* **onJobFinish** fires per member with the exact `amountPaid` (their split of the pool). Each
  member may also receive a **bonus item** on finish — driven by their progression `itemChance`
  and granted automatically by Job Core.

***

## <mark style="color:yellow;">**Events — the client side**</mark>

The core drives the in-world side of a run too. Listen with `JobAPI.On` on the client to spawn
props, blips and targets, and clean them up:

```lua
JobAPI.On('client:jobStarted', function(data) end)  -- a run started for you
JobAPI.On('client:jobEnded',  function(data) end)   -- run ended (finish/cancel/leave)
JobAPI.On('client:syncSession', function(data) end) -- run state changed
```

* **jobStarted** fires when a run begins for you **and on reconnect** (see below). Use it for
  one-off setup or to request any state you need from your own server side.
* **syncSession(data)** is the live picture of the run — reconcile your world to it on every fire:

  ```lua
  data = {
      active      = true,
      objectives  = { { key, label, current, target, completed }, ... },
      team        = { { identifier, name, contribution, present }, ... },
      phase       = { index, total, label, keys, complete }, -- nil if the job has no phases
      pool        = 12000,       -- shared payout pool so far
      yourShare   = 3000,        -- your projected cut
      allComplete = false,
      elapsed     = 145,         -- seconds since the run started
  }
  ```

* **jobEnded(data)** — tear down whatever you spawned. Fires on finish, cancel, leaving the party,
  or disconnect.

{% hint style="info" %}
**Reconnect / anti-crash is built in.** If a player disconnects mid-run and returns within the
configured window, Job Core drops them straight back into the same run and re-fires `jobStarted`
+ `syncSession`. Because you already react to those events, your world simply respawns — no
reconnect code needed on your side.
{% endhint %}

***

## <mark style="color:yellow;">**Your own gameplay events**</mark>

Everything above is the **core's** API. The events *you* invent for your job's own plumbing are
just normal FiveM events — use `RegisterNetEvent` / `TriggerServerEvent` / `TriggerClientEvent`
with your own names. You don't route them through `JobAPI`:

```lua
-- client asks the server to work an objective
TriggerServerEvent('myjob:server:objUse', key)

-- server reacts and credits the shared objective
RegisterNetEvent('myjob:server:objUse', function(key)
    JobAPI.AddObjectiveProgress(source, key, 1)
end)
```

***

## <mark style="color:yellow;">**Putting it together**</mark>

A minimal server-side job that adds an objective when a run starts, credits it from its own
gameplay event, advances phases, and reacts when the crew is done:

```lua
-- inject your objectives when a run starts
JobAPI.On('server:onJobStart', function(source, partyId)
    JobAPI.AppendObjectives(source, {
        { key = 'deliver', label = 'Deliver the package', target = 1, xp = 25, reward = 500 },
    })
end)

-- your own gameplay event credits the objective
RegisterNetEvent('myjob:server:delivered', function()
    JobAPI.AddObjectiveProgress(source, 'deliver', 1)
end)

-- phased job? append the next phase the moment the current one is done
JobAPI.On('server:onPhaseComplete', function(source, phaseIndex)
    local next = JobAPI.GetPhaseObjectives(Config.Job.phases, phaseIndex + 1)
    if next then JobAPI.AppendObjectives(source, next) end
end)

JobAPI.On('server:onAllComplete', function(partyId)
    print('crew finished every objective — send them back to hand in')
end)
```
