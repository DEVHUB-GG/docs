---
description: >-
  Exports and events exposed by Job Core for building jobs, missions and reward
  flows from your own resource — no access to Job Core's files needed.
---

# ⚙️ Exports & Events

Everything you need to integrate Job Core lives here. From **your own resource** you
call its **server exports** to drive a run, and listen to its **server and client
events** to react. You never touch Job Core's files.

A few things that apply everywhere:

* Every **server export** takes the player's server id (`source`) as its first argument.
* "Objective" = a shared, party-pooled counter (`current` / `target`). Any crew member's
  progress counts toward it and is synced to everyone.
* "Run" / "session" = one active job for a party. It ends on **finish**, **cancel**, or
  when the party dissolves.

***

## <mark style="color:yellow;">**Server Exports — run control**</mark>

Start, stop and inspect a party's run. Leader-only calls return `ok, messageKey`.

```lua
exports['devhub_jobCore']:startJob(source)   -- ok, key  — leader only; begin a run for the caller's party
exports['devhub_jobCore']:cancelJob(source)  -- ok, key  — leader only; end the run, no payout
exports['devhub_jobCore']:finishJob(source)  -- ok, key  — leader only; split the pool and pay the crew
exports['devhub_jobCore']:isOnJob(source)    -- bool     — is this player in an active run?
exports['devhub_jobCore']:getScaling(source) -- { size, missions, payout, xp } or nil
```

* **startJob / cancelJob / finishJob**: only the party **leader** may call these; a non-leader
  gets `false` plus a message key. Starting snapshots the party size for the whole run.
* **getScaling**: the scaling locked in at start — `payout` / `xp` are the multipliers applied
  to this run, `missions` is the base objective count, `size` the party size.

***

## <mark style="color:yellow;">**Server Exports — objectives & phases**</mark>

Add objectives to a live run and push progress. `addObjectiveProgress` is the main call you
make from gameplay — it credits the shared counter, grants the contributor XP, tops up the
payout pool, and syncs the whole crew.

```lua
exports['devhub_jobCore']:addObjectiveProgress(source, key, amount) -- bool — add shared progress (xp + pool + sync)
exports['devhub_jobCore']:setObjective(source, key, current)        -- bool — set progress absolutely (no xp/pool)
exports['devhub_jobCore']:appendObjectives(source, objectives)      -- bool — append objectives to the live run
exports['devhub_jobCore']:getObjectives(source)                     -- array of { key, label, current, target, completed } or nil
exports['devhub_jobCore']:getPhase(source)                          -- { index, total, label, keys, complete } or nil
```

* **appendObjectives(source, objectives)**: `objectives` is an array. Each entry:

  ```lua
  {
      key    = 'deliver_1',     -- unique id you use with addObjectiveProgress
      label  = 'Deliver the package', -- shown in the mission HUD
      target = 3,               -- units needed to complete
      xp     = 25,              -- xp per unit to the contributor (optional)
      reward = 500,             -- money per unit into the shared pool (optional)
      -- optional PHASE metadata — drives the HUD phase label + filtering:
      phase  = 1, phaseLabel = 'Gather', phaseTotal = 3,
  }
  ```

* **getPhase(source)**: for phased jobs, the current phase — `index` / `total`, its `label`,
  the objective `keys` in it, and `complete` (all of the phase's objectives done). Returns
  `nil` when the job has no phases. Use it to gate phase-based gameplay. Job Core also fires the
  **onPhaseComplete** event (below) the instant a phase's objectives are all done, so you
  usually react to that instead of polling here.

***

## <mark style="color:yellow;">**Server Exports — quests**</mark>

```lua
exports['devhub_jobCore']:addQuestProgress(source, chainId, amount) -- bool — advance a persistent quest chain
exports['devhub_jobCore']:getQuestProgress(source)                  -- array of quest chains with per-step state
```

* Quests are **persistent** and separate from a run's mission objectives — drive them from any
  gameplay with `addQuestProgress`. Completing a step (or the whole chain) grants its configured
  reward and fires the `Config.OnQuestStep` / `Config.OnQuestComplete` hooks.

***

## <mark style="color:yellow;">**Server Exports — party / crew**</mark>

```lua
exports['devhub_jobCore']:getParty(source)         -- party payload or false
exports['devhub_jobCore']:getPartyMembers(source)  -- array of { source, identifier, name } or nil
exports['devhub_jobCore']:isLeader(source)         -- bool
exports['devhub_jobCore']:getLeader(source)        -- leader's server id or nil
exports['devhub_jobCore']:sendToAllInParty(source, event, ...) -- fire a client event on every online member
```

* **getParty(source)** returns `false` when the player has no party, otherwise:

  ```lua
  { id, youAreLeader, leaderIdentifier, maxSize, customSplit,
    members = { { source, identifier, name, level, isLeader, percent }, ... } }
  ```

* **sendToAllInParty(source, event, ...)**: triggers the given **client** event on every online
  member of the caller's party, forwarding any extra args. Solo = just the caller. Ideal for
  syncing your own gameplay state (e.g. "this prop was taken") to the whole crew:

  ```lua
  exports['devhub_jobCore']:sendToAllInParty(src, 'my_job:client:removeProp', propId)
  ```

***

## <mark style="color:yellow;">**Server Events**</mark>

Listen with `AddEventHandler` on the server to react to a run's lifecycle.

```lua
AddEventHandler('devhub_jobCore:server:onJobStart', function(source, partyId) end)
AddEventHandler('devhub_jobCore:server:onJobCancel', function(source) end)
AddEventHandler('devhub_jobCore:server:onJobFinish', function(source, amountPaid) end)
AddEventHandler('devhub_jobCore:server:onObjectiveUpdate', function(source, key, current, target) end)
AddEventHandler('devhub_jobCore:server:onObjectiveComplete', function(source, key) end)
AddEventHandler('devhub_jobCore:server:onPhaseComplete', function(source, phaseIndex, phase) end)
AddEventHandler('devhub_jobCore:server:onAllComplete', function(partyId) end)
AddEventHandler('devhub_jobCore:server:onLevelUp', function(source, newLevel, oldLevel) end)
```

* **onJobStart** fires once per member with the shared `partyId`. A great spot to inject your
  own objectives with `appendObjectives`.
* **onObjectiveComplete** fires the moment a shared objective is finished. **onPhaseComplete**
  fires once when every objective of the *current phase* is done (phased jobs only) — it's fired
  **before** the run's "all done" check, so appending the next phase's objectives from here keeps
  the run going. **onAllComplete** fires when every objective of the whole run is done.
* **onJobFinish** fires per member with the exact `amountPaid` (their split of the pool). Each
  member may also receive a **bonus item** on finish — driven by their progression `itemChance`
  and granted automatically by Job Core.

***

## <mark style="color:yellow;">**Client Events**</mark>

Register these on the client (`RegisterNetEvent`) to build the in-world side of a job — spawn
props, blips and targets, and clean them up.

```lua
RegisterNetEvent('devhub_jobCore:client:jobStarted', function(data) end)  -- a run started for you
RegisterNetEvent('devhub_jobCore:client:jobEnded',  function(data) end)   -- run ended (finish/cancel/leave)
RegisterNetEvent('devhub_jobCore:client:syncSession', function(data) end) -- run state changed
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

## <mark style="color:yellow;">**Putting it together**</mark>

A minimal companion resource that adds an objective when a run starts, credits it from its own
gameplay, advances phases, and reacts when the crew is done:

```lua
-- SERVER (your resource)
AddEventHandler('devhub_jobCore:server:onJobStart', function(source, partyId)
    exports['devhub_jobCore']:appendObjectives(source, {
        { key = 'deliver', label = 'Deliver the package', target = 1, xp = 25, reward = 500 },
    })
end)

-- ...later, when your gameplay decides the package was delivered:
RegisterNetEvent('my_job:server:delivered', function()
    exports['devhub_jobCore']:addObjectiveProgress(source, 'deliver', 1)
end)

-- phased job? append the next phase the moment the current one is done:
AddEventHandler('devhub_jobCore:server:onPhaseComplete', function(source, phaseIndex, phase)
    -- exports['devhub_jobCore']:appendObjectives(source, objectivesForPhase(phaseIndex + 1))
end)

AddEventHandler('devhub_jobCore:server:onAllComplete', function(partyId)
    print('crew finished every objective — send them back to hand in')
end)
```
