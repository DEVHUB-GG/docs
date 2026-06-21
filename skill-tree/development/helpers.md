# Helpers

### <mark style="color:yellow;">What they are</mark>

`Helpers` is a global server-side table defined in `configs/s.helpers.lua`. It wraps common framework operations (jobs, items, money, identifiers, admin) so your handlers don't have to deal with framework-specific code.

{% hint style="info" %}
**These are NOT exports** — they're globals available inside `Config.UnlockHandlerForSkills`, `Config.CategoryVisibilityHandler`, `Config.PremiumCurrency.handler`, and any other server-side config hook.
{% endhint %}

The underlying framework calls (`Core.GetJob`, `Core.GetItemCount`, etc.) are provided by **devhub\_lib** — so as long as devhub\_lib is configured for your framework, all helpers work the same way regardless of whether you use ESX, QBCore, or a custom core.

***

### <mark style="color:yellow;">Functions</mark>

#### `Helpers.CheckJob(source, job, grade)`

Check if a player has a specific job, optionally with a minimum grade.

**Parameters:**

* `source` (number): Player server ID
* `job` (string): Job name (e.g. `'police'`)
* `grade` (number | nil): Minimum grade required (optional)

**Returns:** `boolean`

```lua
if Helpers.CheckJob(source, 'police', 2) then
    -- player is at least a grade 2 police officer
end
```

***

#### `Helpers.HasItem(source, item, amount)`

Check if the player has at least `amount` of an item in their inventory.

**Parameters:**

* `source` (number): Player server ID
* `item` (string): Item name
* `amount` (number | nil): Amount required (defaults to 1)

**Returns:** `boolean`

```lua
if Helpers.HasItem(source, 'energy_drink', 2) then
    -- player has at least 2 energy drinks
end
```

***

#### `Helpers.CheckIdentifier(source, identifier)`

Check if the player's identifier matches an exact value.

**Parameters:**

* `source` (number): Player server ID
* `identifier` (string): Identifier to match (e.g. `'license:abc...'`)

**Returns:** `boolean`

```lua
if Helpers.CheckIdentifier(source, 'license:abcdef1234567890') then
    -- this is THE player
end
```

***

#### `Helpers.IsAdmin(source)`

Convenience wrapper for `Core.IsPlayerAdmin(source)`.

**Returns:** `boolean`

```lua
if Helpers.IsAdmin(source) then
    -- admin-only branch
end
```

***

#### `Helpers.HasEnoughMoney(source, amount)`

Check if the player has at least `amount` cash on hand.

**Parameters:**

* `source` (number): Player server ID
* `amount` (number): Required amount of cash

**Returns:** `boolean`

```lua
if Helpers.HasEnoughMoney(source, 500) then
    -- player can afford it
end
```

***

#### `Helpers.CheckUnlockedSkill(source, categoryUid, skillUid)`

Check if a specific skill is already unlocked. Useful for chaining unlocks (require a previous skill).

**Parameters:**

* `source` (number): Player server ID
* `categoryUid` (string): Category UID
* `skillUid` (string): Skill UID

**Returns:** `boolean`

```lua
if Helpers.CheckUnlockedSkill(source, 'personal', 'runFaster_1') then
    -- prerequisite met
end
```

***

#### `Helpers.RemoveItem(source, item, amount)`

Remove an item from the player's inventory.

**Parameters:**

* `source` (number): Player server ID
* `item` (string): Item name
* `amount` (number | nil): Amount to remove (defaults to 1)

```lua
Helpers.RemoveItem(source, 'energy_drink', 1)
```

***

#### `Helpers.RemoveMoney(source, amount)`

Remove cash from the player.

**Parameters:**

* `source` (number): Player server ID
* `amount` (number): Amount of cash to remove

```lua
Helpers.RemoveMoney(source, 500)
```

***

### <mark style="color:yellow;">Example — combined unlock requirement</mark>

A skill that requires the player to be a grade 2+ police officer, hold an energy drink, and pay $500 in cash:

```lua
Config.UnlockHandlerForSkills = {
    ['personal'] = {
        ['runFaster_3'] = {
            unlockRequirementMessage = 'Police only — costs 1 energy drink and $500.',
            handler = function(source)
                if not Helpers.CheckJob(source, 'police', 2) then return false end
                if not Helpers.HasItem(source, 'energy_drink', 1) then return false end
                if not Helpers.HasEnoughMoney(source, 500) then return false end

                Helpers.RemoveItem(source, 'energy_drink', 1)
                Helpers.RemoveMoney(source, 500)
                return true
            end,
        },
    },
}
```

***

### <mark style="color:yellow;">Example — premium currency handler</mark>

The same helpers can power a premium currency handler that uses regular cash as the "premium" payment:

```lua
Config.PremiumCurrency = {
    type = "optional",
    label = "$",
    icon = "fas fa-dollar-sign",
    handler = function(amount, categoryUid, skillUid, source)
        if not Helpers.HasEnoughMoney(source, amount) then return false end
        Helpers.RemoveMoney(source, amount)
        return true
    end,
}
```
