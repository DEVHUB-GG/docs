# Listeners

{% hint style="info" %}
**v3** — any listener below can be selectively disabled for optimization via `Config.DisabledListeners`. If you don't react to an event from another resource, disable it to reduce network traffic.
{% endhint %}

### <mark style="color:yellow;">Client Events</mark>

#### Skill Unlock Event

Triggered when a player unlocks a new skill and after player is loaded first time to server. This should be used if you have skill which apply effect one time, its good for optimalization.

```lua
local moreHp = 0
RegisterNetEvent('devhub_skillTree:client:listener:skillUnlocked', function(categoryUid, skillUid, firstUnlock)
    local ped = PlayerPedId()
    local maxHp = GetEntityMaxHealth(ped)
    if categoryUid == 'personal' and skillUid == 'moreHp' then
        moreHp = moreHp + 50
    end
    SetPedMaxHealth(ped, maxHp + moreHp)
    if firstUnlock then
        print("USER UNLOCKED NEW SKILL")
    end
end)
```

**Parameters:**

* `categoryUid` (string): Category name (e.g., 'personal')
* `skillUid` (string): Unique identifier of the unlocked skill
* `firstUnlock` (boolean) It will only be true on skill unlock and when synced on relog it will be nil

This listener will be triggered in following situactions:

* First time skill unlock
* Joining server will skill already unlocked
* Skill recovered from degradation

***

#### XP Gain Event

Triggered when a player receives new XP.

{% hint style="info" %}
XP from the personal category is cached on the client side and refreshed every few minutes
{% endhint %}

```lua
RegisterNetEvent('devhub_skillTree:client:listener:newXp', function(categoryUid, amount)
    exports['your-hud']:ShowNotification({
        type = 'xp',
        message = string.format('+%d XP (%s)', amount, categoryUid),
        duration = 3000
    })
end)
```

**Parameters:**

* `categoryUid` (string): Category name (e.g., 'personal')
* `amount` (number): Amount of XP received

***

#### Skill Reset Event

Triggered when a player resets their skills in a category.

```lua
local effectApplied = true
RegisterNetEvent('devhub_skillTree:client:listener:skillReset', function(categoryUid)
    if categoryUid == 'personal' then
        effectApplied = false
    end
end)
```

**Parameters:**

* `categoryUid` (string): Category name (e.g., 'personal')

***

#### Level Up Event

Triggered when a player levels up in a skill category.

```lua
RegisterNetEvent('devhub_skillTree:client:listener:levelUp', function(categoryUid, newLevel)
    exports['your-hud']:ShowNotification({
        type = 'level',
        message = string.format('Level up! %s reached level %d', categoryUid, newLevel),
        duration = 3000
    })
end)
```

**Parameters:**

* `categoryUid` (string): Category name (e.g., 'personal')
* `newLevel` (number): The new level achieved

***

#### Skill Degraded Event (v3)

Triggered when a skill's degradation XP hits 0 and the skill effect is disabled. See the Skill Degradation page for the full lifecycle.

```lua
RegisterNetEvent('devhub_skillTree:client:listener:skillDegraded', function(categoryUid, skillUid)
    -- Skill effect just got disabled by degradation
    -- e.g. show a warning, remove the buff
end)
```

**Parameters:**

* `categoryUid` (string): Category name (e.g., 'personal')
* `skillUid` (string): Unique identifier of the depleted skill

***

#### Skill Degraded Recovered Event (v3)

Triggered when a previously-depleted skill regains enough degradation XP to reactivate.

```lua
RegisterNetEvent('devhub_skillTree:client:listener:skillDegradedRecovered', function(categoryUid, skillUid)
    -- Skill effect is back online
end)
```

**Parameters:**

* `categoryUid` (string): Category name (e.g., 'personal')
* `skillUid` (string): Unique identifier of the recovered skill

***

### <mark style="color:yellow;">Server Events</mark>

#### Skill Unlock Event

Triggered when a player unlocks a new skill and after player is loaded first time to server. This should be used if you have skill which apply effect one time, its good for optimalization.

```lua
RegisterNetEvent('devhub_skillTree:server:listener:skillUnlocked', function(source, categoryUid, skillUid)
-- do your stuff
end)
```

**Parameters:**

* `source (number)` Player Id
* `categoryUid` (string): Category name (e.g., 'personal')
* `skillUid` (string): Unique identifier of the unlocked skill

This listener will be triggered in following situactions:

* First time skill unlock
* Joining server will skill already unlocked
* Skill recovered from degradation

***

#### Skill Reset Event

Triggered when a player resets their skills in a category.

```lua
local effectApplied = true
RegisterNetEvent('devhub_skillTree:server:listener:skillReset', function(source ,categoryUid)
    if categoryUid == 'personal' then
        effectApplied = false
    end
end)
```

**Parameters:**

* `source (number)` Player Id
* `categoryUid` (string): Category name (e.g., 'personal')

***

#### Skill Degraded Event (v3)

Server-side mirror of the client `skillDegraded` event. Fires when a skill's degradation XP hits 0.

```lua
RegisterNetEvent('devhub_skillTree:server:listener:skillDegraded', function(source, categoryUid, skillUid)
-- do your stuff
end)
```

**Parameters:**

* `source` (number): Player Id
* `categoryUid` (string): Category name (e.g., 'personal')
* `skillUid` (string): Unique identifier of the depleted skill

***

#### Skill Degraded Recovered Event (v3)

Server-side mirror of the client `skillDegradedRecovered` event.

```lua
RegisterNetEvent('devhub_skillTree:server:listener:skillDegradedRecovered', function(source, categoryUid, skillUid)
-- do your stuff
end)
```

**Parameters:**

* `source` (number): Player Id
* `categoryUid` (string): Category name (e.g., 'personal')
* `skillUid` (string): Unique identifier of the recovered skill
