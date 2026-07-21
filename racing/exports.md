---
description: >-
  Integration exports and client visibility hooks for the DevHub Racing script,
  for calling races from your own resource.
---

# 📚 Exports

{% hint style="warning" %}
The client exports (`JoinNearestRace`, `JoinRaceById`) act on the **local player** and are meant for UI, targets, commands and menus. They only send a join **request** - the server still runs every eligibility check (funds, vehicle class, skills, race state) before the player is actually added. Anything authoritative (creating races, granting rewards) must go through the **server** export.
{% endhint %}

***

## <mark style="color:yellow;">**Server Exports**</mark>

### createRace

```lua
local raceId = exports['devhub_racing']:createRace({
    mapId = 1,              -- required: map ID (number) or "random"
    startDelay = 300,       -- optional: seconds until race starts (default: 60)
    laps = 3,               -- optional: laps for loop maps (default: 3)
    collisions = true,      -- optional: player vehicle collisions (default: true)
    fpv = false,            -- optional: force first person view (default: false)
    entryFee = 500,         -- optional: entry fee (default: 0)
    vehicleClass = "ALL",   -- optional: "ALL", "S", "A", "B", "C", "D" (default: "ALL")
    winnerReward = 10000,   -- optional: money reward for winner (default: 0)
    official = true,        -- optional: MMR-ranked race (default: false)
    creatorIdentifier = "SYSTEM", -- optional: creator identifier (default: "SYSTEM")
})
```

* **What it does**: Creates a new race on the server from a map, exactly like a player creating one from the app, and makes it joinable. Use it to script events, auto-scheduled races or third-party integrations.
* **When to use**: From server-side code when you want to open a race without a player going through the map UI.
* **Parameters**: a single `data` table:
  * `mapId` *(number|string, required)* - the map ID to use, or `"random"` for a random verified map.
  * `startDelay` *(number, optional)* - seconds until the race starts (default: `60`).
  * `laps` *(number, optional)* - number of laps for loop maps (default: `3`).
  * `collisions` *(boolean, optional)* - enable player vehicle collisions (default: `true`).
  * `fpv` *(boolean, optional)* - force first person view (default: `false`).
  * `entryFee` *(number, optional)* - entry fee amount (default: `0`).
  * `vehicleClass` *(string, optional)* - class restriction: `"ALL"`, `"S"`, `"A"`, `"B"`, `"C"`, `"D"` (default: `"ALL"`).
  * `winnerReward` *(number, optional)* - money reward for the winner (default: `0`).
  * `official` *(boolean, optional)* - whether the race is official / MMR-ranked (default: `false`).
  * `creatorIdentifier` *(string, optional)* - identifier of the creator (default: `"SYSTEM"`).
* **Returns**: `number|false` - the created race ID, or `false` if the race could not be created (e.g. the map does not exist).

***

## <mark style="color:yellow;">**Client Exports**</mark>

These exist for owners who hide the built-in 3D join prompt via `OpenClientFunctions.ShouldShowJoinUi` and want to trigger joining from their own target/command/menu instead.

| Export | Returns | Purpose |
| --- | --- | --- |
| `JoinNearestRace()` | `boolean` | Join the closest joinable race in range |
| `JoinRaceById(raceId)` | `boolean` | Join a specific race by ID, ignoring distance |

### JoinNearestRace

```lua
local sent = exports['devhub_racing']:JoinNearestRace()
```

* **What it does**: Finds the closest race within the built-in join range and attempts to join it, running the exact same eligibility checks the built-in prompt does. If there is no race nearby, the player gets the `no_race_nearby` notification.
* **When to use**: From your own trigger (a target option, command or menu) when you have hidden the built-in join prompt.
* **Parameters**: none.
* **Returns**: `boolean` - `true` if a join request was sent to the server, `false` if it was not (the player is already in a race or the map creator, or no race is nearby).

### JoinRaceById

```lua
local sent = exports['devhub_racing']:JoinRaceById(raceId)
```

* **What it does**: Attempts to join a specific race by its ID, ignoring proximity, running the same eligibility checks as the built-in prompt. If the ID does not match a known race, the player gets the `race_not_found` notification.
* **When to use**: When your own UI already knows which race the player picked (e.g. a list of nearby races) and you want to join it directly.
* **Parameters**:
  * `raceId` *(number|string, required)* - the ID of the race to join.
* **Returns**: `boolean` - `true` if a join request was sent to the server, `false` if it was not (the player is already in a race or the map creator, or the race ID is unknown).

***

## <mark style="color:yellow;">**Client Open Functions**</mark>

These live in `configs/c.functions.lua` and let you gate what the built-in UI shows per player and per race. Return `false` to hide, `true` (or nothing) to show.

{% hint style="danger" %}
`ShouldShowJoinUi` and `ShouldShowBlip` run **repeatedly** while the player is near a race. Keep them light - no loops, waits or server callbacks inside them, or you will hurt the framerate.
{% endhint %}

| Hook | Fires when | Blocking |
| --- | --- | --- |
| `ShouldShowJoinUi(raceData)` | Every tick while near a race, to decide if the built-in 3D join prompt is drawn | ✅ return `false` to hide it |
| `ShouldShowBlip(raceData)` | While a race blip is being evaluated, to decide if its start blip is shown | ✅ return `false` to hide it |

**Example - only show the join prompt to on-duty racers:**

```lua
OpenClientFunctions.ShouldShowJoinUi = function(raceData)
    return LocalPlayer.state.racingEnabled == true
end
```

When you hide the prompt this way, wire your own trigger to the `JoinNearestRace` / `JoinRaceById` exports so players can still join.

***

## <mark style="color:yellow;">**Related Pages**</mark>

{% content-ref url="configuration.md" %}
[configuration.md](configuration.md)
{% endcontent-ref %}

{% content-ref url="script-flow.md" %}
[script-flow.md](script-flow.md)
{% endcontent-ref %}
