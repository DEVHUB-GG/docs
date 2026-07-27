---
description: Documentation for server.lua Configuration
---

# server.lua

One function, and it ships empty on purpose.

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
