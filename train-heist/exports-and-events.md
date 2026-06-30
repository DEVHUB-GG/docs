---
description: >-
  Server exports and open-function hooks exposed by Train Heist for integrating
  with other resources.
---

# ⚙️ Exports & Events

Train Heist exposes a small set of **server exports** for the queue, plus two open-function hooks — `TriggerAlarm` in `configs/client.lua` and `configs/server.lua` — that you edit to react to police alerts.

## <mark style="color:yellow;">**Server Exports**</mark>

Call these from any server script as `exports['devhub_trainHeist']:<name>(...)`.

```lua
exports['devhub_trainHeist']:AddToQueue(source)        -- Add a player to the heist queue
exports['devhub_trainHeist']:RemoveFromQueue(source)   -- Remove a player from the queue
exports['devhub_trainHeist']:GetQueuePosition(source)  -- A player's queue position, cooldown and rewards flag
exports['devhub_trainHeist']:GetQueueAmount()          -- Number of players currently in the queue
exports['devhub_trainHeist']:SetRewardCollected(source) -- Mark a player's pending laptop rewards as collected
```

* **AddToQueue(source)**: Adds the player to the queue after checking police count, personal cooldown and start requirements. Returns `true, position` on success, or `false, reason` if rejected (`"Already in queue"`, `"On cooldown"`, `"Missing requirement"`, `"Not enough police online"`).
* **RemoveFromQueue(source)**: Removes the player from the queue. Returns `true` on success, or `false, "Not in queue"`.
* **GetQueuePosition(source)**: Returns three values — the player's `position` (or `false`), their remaining `cooldown` in seconds (or `false`), and a `rewards` flag (`true` if they have laptop rewards waiting to collect, otherwise `false`).
* **GetQueueAmount()**: Returns the number of players currently in the queue.
* **SetRewardCollected(source)**: Clears the player's "rewards to collect" flag (called once they claim their post-heist laptop rewards). Returns `true` if there were rewards to clear, otherwise `false`.

***

## <mark style="color:yellow;">**Police Alarm Hooks**</mark>

When a player handles loot and the chance roll succeeds (`Config.Police.callChance`), Train Heist calls the `TriggerAlarm` hook so you can plug in your own dispatch / alarm system. There is one on each side — use whichever fits your dispatch resource; you do not need both.

**Server side** — `configs/server.lua`:

```lua
function TriggerAlarm(source, coords)
    -- Plug your dispatch / alarm logic here (e.g. ps-dispatch, cd_dispatch, ...)
end
```

* `source` is the player who tipped off the cops; `coords` is where they were.

**Client side** — `configs/client.lua`:

```lua
function TriggerAlarm(coords)
    -- Plug your client-side dispatch / alarm logic here
end
```

* Runs on the player who tipped off the cops; `coords` is where they were.

{% hint style="info" %}
Both hooks are empty by default. Implement the alert in **one** of them depending on whether your dispatch resource expects to be triggered server-side or client-side.
{% endhint %}
