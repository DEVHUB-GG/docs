# Example skill

## <mark style="color:yellow;">Adding XP to Skills</mark>

### Client Side

```lua
-- Add XP to specific category
exports['devhub_skillTree']:addXp('personal', 100)

-- Add XP on action
RegisterNetEvent('minigame:lockpickSuccess', function()
    exports['devhub_skillTree']:addXp('thief', 50)
end)
```

***

### Server Side

```lua
-- Add XP to player's skill
exports['devhub_skillTree']:addXp('personal', 100, source)

-- Example: Add XP when player sells items
RegisterNetEvent('inventory:sellItems', function(items)
    local src = source
    local xpGain = #items * 10
    exports['devhub_skillTree']:addXp('thief', xpGain, src)
end)
```

***

## <mark style="color:yellow;">Checking Skill Status</mark>

### Client Side

```lua
-- Check if player has unlocked specific skill
if exports['devhub_skillTree']:hasUnlockedSkill('personal', 'runFaster_1') then
    -- Apply speed boost
end

-- Get skill effect value
local effect = exports['devhub_skillTree']:getSkillEffect('personal', 'runFaster_1')
print('Speed boost multiplier:', effect)

```

***

### Server Side

```lua
-- Check if player has unlocked specific skill
if exports['devhub_skillTree']:hasUnlockedSkill('personal', 'xp_1', source) then
    -- Apply more xp
end

-- Get skill effect value
local effect = exports['devhub_skillTree']:getSkillEffect('personal', 'xp_1', source)
print('Add more xp:', effect)

```

***

## <mark style="color:yellow;">Event Listeners</mark>

### Basic Health Boost System

```lua
RegisterNetEvent('devhub_skillTree:client:listener:skillUnlocked', function(skill, uid)
    if skill == 'test' and uid == 'more_hp' then
        local moreHp = 200
        local effect = exports['devhub_skillTree']:getSkillEffect(skill, uid) or 0
        moreHp = moreHp + effect
        SetEntityMaxHealth(PlayerPedId(), moreHp)
    end
end)
```

***

### Progressive Running Speed System

```lua
local baseSpeed = 1.0
local currentSpeedBoost = 0.0

RegisterNetEvent('devhub_skillTree:client:listener:skillUnlocked', function(skill, uid)
    if skill == 'personal' and string.find(uid, 'runFaster_') then
        local effect = exports['devhub_skillTree']:getSkillEffect(skill, uid) or 0
        currentSpeedBoost = currentSpeedBoost + (effect * 0.1) -- 10% per level
        SetRunSprintMultiplierForPlayer(PlayerId(), baseSpeed + currentSpeedBoost)
    end
end)
```

***

## <mark style="color:yellow;">Skill Checks</mark>

### Basic Bank Access

```lua
RegisterCommand('bank', function()
    local isAllowed = exports['devhub_skillTree']:hasUnlockedSkill('test', 'bank_access') or false
    if isAllowed then
        print('SKILL UNLOCKED')
    else
        print('DO NOT HAVE SKILL')
    end
end)
```

***

## <mark style="color:yellow;">XP and Points</mark>

### XP Reward System

```lua
-- Reward XP for successful robberies
RegisterNetEvent('robbery:completed', function(difficulty)
    local baseXP = 100
    local xpMultiplier = {
        ['easy'] = 1,
        ['medium'] = 2,
        ['hard'] = 3
    }

    local xpAmount = baseXP * (xpMultiplier[difficulty] or 1)
    exports['devhub_skillTree']:addXp('criminal', xpAmount)
end)
```

***

### Points Distribution System

```lua
-- Award skill points for completing missions
RegisterNetEvent('mission:completed', function(missionType)
    local pointRewards = {
        ['heist'] = 2,
        ['delivery'] = 1,
        ['assassination'] = 3
    }

    local points = pointRewards[missionType] or 1
    exports['devhub_skillTree']:addPoints('personal', points)
end)
```

***

## <mark style="color:yellow;">Complete Systems</mark>

### Using Listener

Progressive Weapon Handling System&#x20;

```lua
local weaponSkills = {
    accuracy = 0,
}

-- Listen for skill unlocks
RegisterNetEvent('devhub_skillTree:client:listener:skillUnlocked', function(skill, uid)
    if skill ~= 'combat' then return end

    if uid == 'accuracy_master' then
        local effect = exports['devhub_skillTree']:getSkillEffect(skill, uid)
        weaponSkills.accuracy = effect
    end

    UpdateWeaponHandling()
end)

-- Apply weapon handling modifications
function UpdateWeaponHandling()
    local ped = PlayerPedId()
    local weapon = GetSelectedPedWeapon(ped)

    -- Apply accuracy bonus
    SetPedAccuracy(ped, 100 - weaponSkills.accuracy)
end
```

***

### Using Exports

Swimming Skill Progression System&#x20;

```lua
local swimLevel = 0
local xpGainTimeout = false

-- Check for swimming and award XP
CreateThread(function()
    while true do
        local ped = PlayerPedId()

        if IsPedSwimming(ped) then
            -- Check if player has unlocked swimming skills
            local hasBasicSwim = exports['devhub_skillTree']:hasUnlockedSkill('personal', 'swim_basic')
            local hasAdvancedSwim = exports['devhub_skillTree']:hasUnlockedSkill('personal', 'swim_advanced')

            if hasBasicSwim and not xpGainTimeout then
                -- Award XP for swimming
                exports['devhub_skillTree']:addXp('personal', 5)

                -- Set cooldown
                xpGainTimeout = true
                SetTimeout(10000, function()
                    xpGainTimeout = false
                end)

                -- Apply swimming speed boost based on skills
                local swimSpeed = 1.0
                if hasBasicSwim then
                    local basicEffect = exports['devhub_skillTree']:getSkillEffect('personal', 'swim_basic')
                    swimSpeed = swimSpeed + (basicEffect/100)
                end
                if hasAdvancedSwim then
                    local advancedEffect = exports['devhub_skillTree']:getSkillEffect('personal', 'swim_advanced')
                    swimSpeed = swimSpeed + (advancedEffect/100)
                end

                SetSwimMultiplierForPlayer(PlayerId(), swimSpeed)
            end
        end

        Wait(1000)
    end
end)
```

***

## <mark style="color:yellow;">Ensure correct skill trigger order</mark>

In certain situations, it's essential to ensure that skills are triggered in the correct order. For example, when adding more HP to a player, you might use one of the following design patterns.

### Using Config.SkillLoadingOrder&#x20;

This configuration allows you to define a custom order for loading events when a player first joins the server. Since player data is stored as a hashmap in the database, the loading order of events cannot be naturally controlled.

By default, all events are loaded in an arbitrary order. However, in certain cases, you may need to ensure that specific events are executed before others to properly overwrite values or establish dependencies.

```lua
Config.SkillLoadingOrder = {
    ['more_hp_1'] = 1,
    ['more_hp_2'] = 2,
    ['more_hp_3'] = 3,
    ['more_hp_4'] = 4,
}
```

***

### Skill-Based Incremental Effect Pattern

#### Overview

This pattern is designed to streamline the process of applying skill effects dynamically without requiring explicit values for each skill level. Instead of defining fixed values per skill tier (e.g., `more_hp_1` → +120 HP, `more_hp_2` → +140 HP), we apply a consistent incremental effect across all levels of the same skill category.

#### Benefits

* **Scalability:** New skill levels can be added without modifying the logic.
* **Simplicity:** Eliminates the need for redundant configuration per skill level.
* **Consistency:** Ensures all skill instances contribute equally.

This pattern is particularly useful for **stat-based skill progression**, where effects scale in a linear or predictable manner.

#### Example Usage

```lua
local currentHp = 100

function getSkillUidNoNumber(uid)
    return uid:gsub('_%d+', '')
end

function applySkill(uid)
    if getSkillUidNoNumber(uid) == "more_hp" then
        currentHp = currentHp + 20
    end
end

applySkill("more_hp_3")
applySkill("more_hp_2")
applySkill("more_hp_4")
applySkill("more_hp_1")

print(currentHp) -- Output: 180
```

