---
description: Documentation for server.lua Configuration
---

# server.lua

Three functions, all shipped empty on purpose. They are the places where the gym hands control over to your own resources.

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
