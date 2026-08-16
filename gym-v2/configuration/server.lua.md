---
description: Documentation for server.lua Configuration
---

# server.lua

Every function in this file ships empty on purpose. They are the places where the gym hands control over to your own resources.

| Function | What it is for |
| --- | --- |
| `OpenStash` | Opening a stash interaction point in your inventory. |
| `OnGymPlayerLoaded` | Re-applying your own effects for a player's Strength and Agility on join. |
| `OnStatLevelUp` | The same effects, the moment a stat levels. |
| `CanStartExercise` | Blocking a set before it starts. |
| `OnExerciseStarted` | Reacting to a set that is now running. |
| `OnExerciseFinished` | Reacting to a set that has ended, however it ended. |

To push something into the gym from the outside instead, for example handing a player a membership, see [exports.md](../exports.md "mention").

## <mark style="color:yellow;">**Open Stash**</mark>

```lua
---@param source number  Player server id
---@param stashId string  Stable stash identifier
function OpenStash(source, stashId)
    -- Open your inventory stash for this player here, keyed by stashId. Until this is
    -- filled in, the stash and boss stash interaction points do nothing.
end
```

* **Description**: Called on the **server** when a player uses a **Stash** or **Boss Stash** interaction point. Every inventory resource opens a stash differently, so this is left to you.
* **When it fires**: only from those two interaction point modules, which belong to the business module. If you do not run businesses, you never need this.
* **stashId**: a stable identifier generated when the interaction point is saved. It never changes for that point, so it is safe to use as the stash key.

**Example** with `ox_inventory`:

```lua
function OpenStash(source, stashId)
    exports.ox_inventory:RegisterStash(stashId, 'Gym Stash', 50, 100000)
    TriggerClientEvent('ox_inventory:openInventory', source, 'stash', stashId)
end
```

**Example** with `qb-inventory`:

```lua
function OpenStash(source, stashId)
    TriggerClientEvent('inventory:client:SetCurrentStash', source, stashId)
    TriggerEvent('qb-inventory:server:OpenInventory', source, 'stash', stashId, {
        maxweight = 100000,
        slots = 50,
    })
end
```

{% hint style="info" %}
Leave it empty if you do not want stashes at the gym. Nothing else depends on it, and the interaction points simply do nothing when used.
{% endhint %}

***

## <mark style="color:yellow;">**Gym stat hooks**</mark>

Two functions that let you tie your own effects to a player's gym progress, for example a bigger inventory at higher Strength. The gym already grants bonus max health and faster stamina recovery from these stats on its own, see [gym-config.md](../gym-creator/gym-config.md); these hooks are for everything else.

### The data both hooks receive

Both are handed the same `stats` table, so one function can serve them both:

```lua
stats.strength.level      -- current level, starts at 1
stats.strength.xp         -- XP inside that level
stats.strength.xpToNext   -- XP needed for the next level
stats.agility.level       -- the same three fields
stats.agility.xp
stats.agility.xpToNext
```

**Strength** levels on reps that hit the upper body, **Agility** on reps that hit the lower body and core. Which muscles an exercise works, and how much each level is worth, are both set in the Gym Creator.

### OnGymPlayerLoaded

```lua
---@param source number  Player server id
---@param identifier string  Framework identifier
---@param stats table
function OnGymPlayerLoaded(source, identifier, stats)
end
```

* **When it fires**: once per join, for **every** player, including one who has never set foot in a gym. In that case the levels are simply 1, which is what you want: your handler can be a plain "set the state from the level" with no special case for a fresh player.
* **Why you need it**: a level is a state, not an event. Without this hook, a player who levelled up yesterday would join today with your effect missing until they happen to level again.

{% hint style="warning" %}
This runs the moment the player's gym data is ready, which may be before your inventory resource has finished loading them. If yours needs longer, add your own `Wait` inside the function.
{% endhint %}

### OnStatLevelUp

```lua
---@param source number  Player server id
---@param identifier string  Framework identifier
---@param stats table  Already holding the new levels
function OnStatLevelUp(source, identifier, stats)
end
```

* **When it fires**: whenever Strength or Agility gains a level, so usually in the middle of a workout. Both stats can level in the same set, and the hook fires once for that set with both new values already in `stats`.

### Example

Because both hooks take the same table, write the logic once:

```lua
local function applyGymPerks(source, stats)
    local slots = 40 + math.floor(stats.strength.level / 5)
    exports.ox_inventory:SetPlayerInventorySlots(source, slots)
end

function OnGymPlayerLoaded(source, identifier, stats)
    applyGymPerks(source, stats)
end

function OnStatLevelUp(source, identifier, stats)
    applyGymPerks(source, stats)
end
```

{% hint style="info" %}
Both hooks are called inside a `pcall`, so an error in your code is logged and the gym carries on. It will not break a player's join or interrupt their workout.
{% endhint %}

***

## <mark style="color:yellow;">**Exercise hooks**</mark>

Three functions that wrap a workout set: one gate before it starts, one signal when it does, one when it ends. All three are optional. Delete any of them and the gym behaves exactly as it did before.

### The data table all three receive

They share one `data` table, so a single helper can serve all three. The fields marked **end only** are filled in for `OnExerciseFinished` and are absent in the other two.

| Field | Meaning |
| --- | --- |
| `data.exerciseId` | What is being performed, for example `bench_press`. |
| `data.eqId` | Id of the placed machine, unique inside its zone. |
| `data.zoneId` | Gym zone id, or the string `global` for a world prop. |
| `data.gymId` | The same zone as a number, `nil` for global props. |
| `data.gymName` | Gym name, `nil` for global props. |
| `data.businessId` | The business the gym is linked to, `nil` when it is standalone. |
| `data.bodyPart` | Primary muscle worked, one of the body parts from [gym-config.md](../gym-creator/gym-config.md "mention"). |
| `data.maxReps` | Rep target of the exercise, `nil` when it has none. |
| `data.global` | `true` for props enabled on the creator's [global-models.md](../gym-creator/global-models.md "mention") page. |
| `data.reps` | **End only.** Reps that counted, already validated server side. |
| `data.freeReps` | **End only.** How many of those a spotter granted. |
| `data.xp` | **End only.** Skill tree XP granted for the set, `0` without `devhub_skillTree`. |
| `data.statLevels` | **End only.** `{ strength = n, agility = n }`, the levels gained this set. |
| `data.durationMs` | **End only.** How long the set lasted. |
| `data.success` | **End only.** `true` only when the rep target was reached. |
| `data.reason` | **End only.** Why it ended, see the list below. |

{% hint style="info" %}
`data.reps` is the count the server accepted, not the count the client claimed. Reps are counted client side, so the server clamps the report against the exercise's rep cap and the time the set actually took. Use this number for rewards and never trust a higher one.
{% endhint %}

### CanStartExercise

```lua
---@param source number  Player server id
---@param identifier string  Framework identifier
---@param data table
---@return boolean allowed, string|nil reason  The reason is shown to the player
function CanStartExercise(source, identifier, data)
    return true
end
```

* **When it fires**: the last gate before a set starts, **after** the membership and fatigue checks the gym runs itself. A handler here only ever sees sets the gym would otherwise allow.
* **Blocking**: return `false` to refuse, with an optional message that is shown to the player. Anything other than `false` lets the set through, so a function that forgets to return a value does not accidentally lock the gym.

```lua
function CanStartExercise(source, identifier, data)
    if IsPlayerCuffed(source) then
        return false, "Not with those on"
    end
    return true
end
```

### OnExerciseStarted

```lua
---@param source number  Player server id
---@param identifier string  Framework identifier
---@param data table
function OnExerciseStarted(source, identifier, data)
end
```

* **When it fires**: the player passed every gate and the set is running. Somewhere to start a sweat effect, tell a stamina script to back off, or log who trains where.

### OnExerciseFinished

```lua
---@param source number  Player server id
---@param identifier string  Framework identifier, still valid for an offline player
---@param data table  End-only fields included
function OnExerciseFinished(source, identifier, data)
end
```

* **When it fires**: exactly once per set, whatever ended it. Anything you started in `OnExerciseStarted` always gets its counterpart, including when the player disconnects mid rep.

`data.reason` is one of:

| Reason | What happened |
| --- | --- |
| `max_reps` | The rep target was reached. This is the only reason where `data.success` is `true`. |
| `esc_cancel` | The player stopped the set themselves. |
| `minigame_fail` | A rep minigame was lost. |
| `fatigue_overload` | The trained muscle hit its limit mid set. |
| `config_stop` | The per-rep hook in `configs/client.lua` returned `false`. |
| `client_stop` | Any other stop reported by the client. |
| `player_dropped` | The player left while exercising. |
| `resource_stop` | The gym resource was stopped or restarted. |

{% hint style="warning" %}
The last two arrive with `data.reps` at `0`, because a set is only counted once the client reports it, and by then the player may already be gone. Write to the database by `identifier` in those cases rather than doing anything with `source`.
{% endhint %}

### Example

Because all three take the same table, the interesting part is usually one helper:

```lua
function CanStartExercise(source, identifier, data)
    -- No training in the middle of a robbery.
    if exports['my_heists']:IsPlayerBusy(source) then
        return false, "Later. You have somewhere to be."
    end
    return true
end

function OnExerciseStarted(source, identifier, data)
    TriggerClientEvent('my_effects:sweat', source, true)
end

function OnExerciseFinished(source, identifier, data)
    TriggerClientEvent('my_effects:sweat', source, false)

    if data.reps > 0 then
        print(('%s did %d reps of %s at %s')
            :format(identifier, data.reps, data.exerciseId, data.gymName or 'a world prop'))
    end
end
```

{% hint style="info" %}
All three run inside a `pcall`, so an error in your code is written to the console and the gym carries on. It cannot strand a player in a set that never ends.
{% endhint %}

***

## <mark style="color:yellow;">**Membership exports**</mark>

Hooks let you react to what the gym does. To push something into it from your own resource, granting a pass, taking one away or checking one, use the three server exports on [exports.md](../exports.md "mention").
