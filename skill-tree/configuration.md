# 🛠️ Configuration

### <mark style="color:yellow;">Main Configuration (sh.main.lua)</mark>

#### Basic Settings

```lua
Config.MenuCommand = "skill"  -- Command to open menu (string or false)
Config.Keybind = "F7"        -- Keybind to open menu (string or false) 
Config.Item = false          -- Item required to open menu (string or false)
--client event | devhub_skillTree:client:openSkillTree
```

***

#### Theme

Sets the visual theme used by the UI.

```lua
Config.Theme = "modern" -- string | "legacy" | "modern" | "zombie" | "fantasy"
```

***

#### XP Settings

```lua
Config.XpBoost = 1.0         -- XP multiplier
Config.BaseXp = 100          -- Base XP required per level
Config.LevelBasedXp = 50     -- Additional XP per level
Config.PointsPerLevel = 1    -- Skill points awarded per level

function xpAlgorithm(level)
    return Config.BaseXp + (Config.LevelBasedXp * level)
end
```

***

#### System Settings

```lua
Config.CloseUiOnDeath = true         -- Close UI on player death
Config.HpRegenerationTimeout = 5000   -- HP regen cooldown (ms)
Config.EnableGenerator = true         -- Enable skill tree generator (Exclusive only)
Config.EarnXpTick = 1000             -- XP earn check interval (ms)
Config.DisableXpEarnWhileDead = false  -- Disable XP earning while dead
```

***

#### XP Earning Activities

{% hint style="warning" %}
**Format changed in v3.** The amount of XP is now defined directly in `addTo` as the value. The old top-level `xp` field is no longer read. If you keep the v2 format (`addTo = {['personal'] = true}`), the script will silently fall back to **5 XP** per tick.
{% endhint %}

```lua
Config.EarnXpTick = 1000 -- int | Tick interval in ms (keep >= 1000)

Config.EarnXp = {
    ['running']      = { timeout = 10000, addTo = { ['personal'] = 5 } },
    ['swimming']     = { timeout = 10000, addTo = { ['personal'] = 5 } },
    ['melee']        = { timeout = 10000, addTo = { ['personal'] = 5 } },
    ['shooting']     = { timeout = 10000, addTo = { ['personal'] = 5 } },
    ['driving']      = { timeout = 10000, addTo = { ['personal'] = 5 } },
    -- new in v3
    ['climbing']     = { timeout = 10000, addTo = { ['personal'] = 5 } },
    ['parachuting']  = { timeout = 15000, addTo = { ['personal'] = 5 } },
    ['flying']       = { timeout = 10000, addTo = { ['personal'] = 5 } },
    ['boating']      = { timeout = 10000, addTo = { ['personal'] = 5 } },
    ['reloading']    = { timeout = 10000, addTo = { ['personal'] = 5 } },
    ['takingDamage'] = { timeout = 15000, addTo = { ['personal'] = 5 } },
}
```

To disable an activity, simply remove it from the table.

***

#### Daily XP Limit

Caps how much XP a player can earn per day in each category. Resets at the configured hour in server-local time. Set `Config.DailyXpLimit = nil` (or remove the block) to disable entirely.

```lua
Config.DailyXpLimit = {
    ResetHour = 0,         -- int (0-23) | Hour when the daily XP resets (0 = midnight)
    Limits = {
        -- ['personal'] = 5000,
    },
    DefaultLimit = 0,      -- int | Default limit for categories not listed above (0 = unlimited)
}
```

***

#### Skill Degradation

Optional per-skill mechanic. When enabled on a skill, the player must continuously earn XP in that category to keep the skill effect active. Set `Config.SkillDegradation = nil` to disable the system entirely.

```lua
Config.SkillDegradation = {
    CycleInterval   = 1,   -- float | Cycle interval in days (1 = every 24h)
    MaxMissedCycles = 30,  -- int   | Max cycles applied at once when reconnecting after offline time
    Categories      = {},  -- per-category overrides
}
```

***

#### Premium Currency

Optional global secondary currency for unlocking skills. Each skill in the generator can specify a `premiumCurrency` amount.

* `type = "optional"` → if the player pays the premium currency, `points` and `UnlockHandlerForSkills` checks are skipped
* `type = "required"` → premium currency is an additional requirement on top of points / unlock handlers

```lua
Config.PremiumCurrency = {
    type = "optional",   -- "optional" or "required"
    label = "PC",
    icon = "fas fa-coin",
    handler = function(amount, categoryUid, skillUid, source)
        -- Check if player has enough currency, deduct it, return true on success
        if not Helpers.HasEnoughMoney(source, amount) then return false end
        Helpers.RemoveMoney(source, amount)
        return true
    end,
}
```

{% hint style="info" %}
The handler runs **server-side** and receives `(amount, categoryUid, skillUid, source)`. It MUST return `true` on success or `false` on failure.
{% endhint %}

Set `Config.PremiumCurrency = nil` to disable the system.

***

#### Trigger Events On Skill Unlock

Automatically fire your own events whenever a player unlocks a specific skill — no listener boilerplate required.

```lua
Config.TriggerOnSkillUnlock = {
    ['personal'] = {
        ['runFaster_1'] = {
            client = 'myResource:client:onRunFasterUnlock',
            server = 'myResource:server:onRunFasterUnlock',
        },
    },
}
```

* **client** event is triggered with `(category, skillUid, effect)`
* **server** event is triggered with `(source, category, skillUid, effect)`

Both `client` and `server` are optional — set only the one you need.

***

#### Disabled Listeners

Disable specific listener events for optimization. If no other resource listens to a given event, you can disable it to reduce network traffic.

```lua
Config.DisabledListeners = {
    newXp                  = { client = true },
    levelUp                = { client = false },
    skillUnlocked          = { client = false, server = false },
    skillReset             = { server = false }, -- client listener always stays enabled
    skillDegraded          = { client = false, server = false },
    skillDegradedRecovered = { client = false, server = false },
    -- Legacy event names (dh_skillTree:* prefix)
    oldSkillUnlocked       = { client = true },
    oldSkillReset          = { client = true },
}
```

***

#### Config Backup System

{% hint style="info" %}
This setting only has effect in the <mark style="color:green;">**exclusive version**</mark> because backups are tied to generator saves.
{% endhint %}

```lua
Config.MaxBackups = 5 -- int | Maximum number of config backups to keep (0 = unlimited)
```

Automatic backups are created before each generator save. Manual backups can also be created from the admin panel. When the limit is reached, the oldest backup is removed automatically.

***

#### Reset System

```lua
Config.SkillReset = {
    Enabled = false,         -- Enable skill reset feature
    PaymentType = false,     -- Payment method ("cash"/"item"/false)
    Cash = 1000,            -- Reset cost if using cash
    Item = {
        Name = "cash",      -- Required item name
        Amount = 1000       -- Required item amount
    },
    type = 'reset', -- string | "reset" or "clear" , reset = reset skills but points , clear = clear skills, points, xp, level
}
```

***

#### Level & Skills Configuration

```lua
Config.MaxLevel = {
    ['personal'] = 7,    -- Max level per skill tree
}

Config.SkillLoadingOrder = {
    -- Control order of skill loading
    ['skillName'] = 1,
}

Config.MeleeWeapons = {
    -- List of weapons affected by melee damage skills
    ["weapon_hammer"] = true,
    -- ...other melee weapons
}

Config.SkillDefaultValues = {
    ['moreStamina'] = 100.0,
    ['moreMaxHp'] = 200,
    ['timeUnderwater'] = 10.0,
}

Config.DisableDefaultSkillEffects = {
    ['runFaster'] = false,
    ['swimFaster'] = false,
    ['moreStamina'] = false,
    ['timeUnderwater'] = false,
    ['moreMaxHp'] = false,
}

Config.DisableRefreshOnPedOrPidChanged = false

Config.XpCacheLimit = 15 -- int | Used for xp virtualization in all categories from Config.EarnXp, how many ticks should be cached before sending to server
-- How it works? When player earns xp, it is cached in client side and sent to server only when player has enough ticks to send

```

#### **Custom Skill Unlock Requirements**

```lua
Config.UnlockHandlerForSkills = {
    ['CATEGORY_UID'] = { -- skill category uid
        ['SKILL_UID'] = {
            unlockRequirementMessage = 'You need to have energy drink to unlock this skill', -- string | Message shown when unlock fail and when they inspect skill in ui
            handler = function(source)
                -- this is triggered server side
                -- return false to prevent unlocking the skill
                -- to use your core in here set it up in /configs/s.main.lua
                return false
            end
        },
        ['SKILL_UID_1,SKILL_UID_2'] = { -- multiple skills in one handler, separated by comma ","
            unlockRequirementMessage = 'You need to have energy drink to unlock this skill', -- string | Message shown when unlock fail and when they inspect skill in ui
            handler = function(source)
                -- this is triggered server side
                -- return false to prevent unlocking the skill
                -- to use your core in here set it up in /configs/s.main.lua
                return false
            end
        },
    },
}
```

***

### <mark style="color:yellow;">Language Configuration (sh.lang.lua)</mark>

The language file contains all text strings used in the UI. Each string can be customized:

```lua
Config.Lang = {
    ['skill_already_unlocked'] = "Skill already unlocked",
    ['not_enough_points'] = "You don't have enough points",
    -- ...more translations
}
```

***

### <mark style="color:yellow;">Server Configuration (s.main.lua)</mark>

#### Admin Commands

{% hint style="info" %}
Enabled by default in v3 (commented out in v2). Permission is checked via `Core.IsPlayerAdmin(source)` from `devhub_lib`.
{% endhint %}

```lua
-- /addXp <playerId> <categoryUid> <amount>
-- /addPoints <playerId> <categoryUid> <amount>
```

You can also use the in-game admin panel for a UI-driven workflow.

#### Category visibility

```lua
Config.CategoryVisibilityHandler = {
    ['personal'] = function(source)
        -- Return false to hide category
        return true
    end,
}

```

#### Logging Configuration

```lua
Config.Logs = {
    ['addXp'] = "webhook_url",            -- Log XP additions
    ['addPoints'] = "webhook_url",        -- Log point additions
    ['removePoints'] = "webhook_url",     -- Log point removals
    ['removeXp'] = "webhook_url",         -- Log XP removals
    ['levelUp'] = "webhook_url",          -- Log level ups
    ['unlockSkill'] = "webhook_url",      -- Log skill unlocks
    ['skillReset'] = "webhook_url",       -- Log skill resets
    -- Security logs
    ['suspiciousActivityLowPriority'] = "webhook_url",   -- Logs for potential cheating might also be issues (lag/connection)
    ['suspiciousActivityHighPriority'] = "webhook_url"   -- Logs for most likely cheating attempts
}
```

#### Security Settings

{% hint style="warning" %}
It is highly advised to keep this option enabled and transition scripts to utilize server-side exports only.
{% endhint %}

```lua
Config.DisableSensitiveClientExports = true  -- Disable client-side sensitive exports
```

**Suspicious Activity Handler**

```lua
Config.SuspiciousActivity = function(source, privateReason, priority)
    -- Called when suspicious activity is detected
    -- priority: 
        -- 'high' (likely cheating)
        -- 'low' (cheater or possible connection issues)
} 
```

#### Enable Unclock Skill Export

```lua
Config.TurnOnUnlockSkillExport = false -- bool
```

Turn on export for unlocking skills, this will allow you to unlock skills from other scripts, it will skip all requirements. Use it with caution! It might be used in a malicious way or cause script to not work properly if you are not careful. Try unlocking only skills that have active connection to other unlocked skills.

***

### <mark style="color:yellow;">Skills Configuration (sh.skills.lua)</mark>

#### Categories

```lua
Config.SkillsCategory = {
    { skill = 'personal', title = 'Personal', group = nil, icon = nil },
    -- { skill = 'police',  title = 'Police',   group = 'Jobs', icon = 'fas fa-shield' },
}
```

* `skill` (string): category UID
* `title` (string): display name
* `group` (string | nil): _**v3**_ — categories sharing the same `group` string are visually grouped under one tab in the menubar
* `icon` (string | nil): _**v3**_ — optional FontAwesome icon class displayed next to the category name

#### Skills

Each skill is defined with:

* `uid`: Unique identifier
* `title`: Display name
* `icon`: FontAwesome icon or image URL
* `effect`: Numerical effect value
* `description`: Skill description
* `points`: Points required to unlock
* `img`: Preview image/gif URL
* `lines`: Connection lines to other skills
* `index`: Position in skill grid (19 columns, rows are dynamic — minimum 9, auto-expands)
* `degradation` _(v3, optional)_: per-skill degradation overrides — see Skill Degradation

Example skill entry:

```lua
{
    description = "Increases running speed by 3%",
    icon = "fa-solid fa-person-running-fast",
    points = 1,
    uid = "runFaster_1", 
    img = "https://example.com/run.gif",
    lines = { top = false, leftTop = false },
    title = "Speed Boost I",
    index = 139,
    effect = 1,
    -- optional v3 degradation override
    -- degradation = {
    --     maxDegradationXp = 1000,      -- Maximum degradation XP (starts full on unlock)
    --     removePerCycle = 100,         -- XP removed each cycle
    --     reactivationThreshold = 500,  -- XP needed to reactivate after depletion
    -- },
}
```

***

### <mark style="color:yellow;">Helpers (s.helpers.lua)</mark>

{% hint style="info" %}
New in v3. Server-side global utility functions available inside `Config.UnlockHandlerForSkills`, `Config.CategoryVisibilityHandler`, `Config.PremiumCurrency.handler`, and any other server-side hook.
{% endhint %}

```lua
Helpers.CheckJob(source, job, grade)            -- bool : has job (and optional minimum grade)
Helpers.HasItem(source, item, amount)           -- bool : has at least `amount` of item (defaults to 1)
Helpers.CheckIdentifier(source, identifier)     -- bool : player identifier matches
Helpers.IsAdmin(source)                         -- bool : admin permission
Helpers.HasEnoughMoney(source, amount)          -- bool : has enough cash
Helpers.CheckUnlockedSkill(source, cat, skill)  -- bool : skill already unlocked
Helpers.RemoveItem(source, item, amount)        -- void : removes item (defaults to 1)
Helpers.RemoveMoney(source, amount)             -- void : removes cash
```

Example — using helpers in an unlock handler:

```lua
Config.UnlockHandlerForSkills = {
    ['personal'] = {
        ['runFaster_1'] = {
            unlockRequirementMessage = 'You need to be a police officer and have an energy drink.',
            handler = function(source)
                if not Helpers.CheckJob(source, 'police', 2) then return false end
                if not Helpers.HasItem(source, 'energy_drink', 1) then return false end
                Helpers.RemoveItem(source, 'energy_drink', 1)
                return true
            end,
        },
    },
}
```

***

### <mark style="color:yellow;">Ui Configuration (config.js)</mark>

{% hint style="info" %}
W**here to find:** html/config.js
{% endhint %}

```javascript
window.config = {
    numberFormatting: [/\B(?=(\d{3})+(?!\d))/g, " "],
    soundVolume: 0.25,
};
```

***

### <mark style="color:yellow;">Used Natives (c.natives.lua)</mark>

```lua
-- In this file you can add your anti-cheat exports or switch used native to an export that is used in your other scripts.
NATIVES =  {
    SetRunSprintMultiplierForPlayer = function(player, multiplier)
        SetRunSprintMultiplierForPlayer(player, multiplier)
        -- debug
        debugPrint('SetRunSprintMultiplierForPlayer called with multiplier:', multiplier)
    end,
    SetSwimMultiplierForPlayer = function(player, multiplier)
        SetSwimMultiplierForPlayer(player, multiplier)
        -- debug
        debugPrint('SetSwimMultiplierForPlayer called with multiplier:', multiplier)
    end,
    SetPlayerMaxStamina = function(player, maxStamina)
        SetPlayerMaxStamina(player, maxStamina)
        -- debug
        debugPrint('SetPlayerMaxStamina called with maxStamina:', maxStamina)
    end,
    SetPedMaxTimeUnderwater = function(ped, maxTimeUnderwater)
        SetPedMaxTimeUnderwater(ped, maxTimeUnderwater)
        -- debug
        debugPrint('SetPedMaxTimeUnderwater called with maxTimeUnderwater:', maxTimeUnderwater)
    end,
    SetPedMaxHealth = function(ped, maxHealth)
        SetPedMaxHealth(ped, maxHealth)
        -- debug
        debugPrint('SetPedMaxHealth called with maxHealth:', maxHealth, GetPedMaxHealth(ped))
    end,
    SetWeaponDamageModifier = function(weapon, damageModifier)
        SetWeaponDamageModifier(weapon, damageModifier)
        -- debug
        debugPrint('SetWeaponDamageModifier called with weapon:', weapon, 'and damageModifier:', damageModifier)
    end,
    SetPlayerStamina = function(player, stamina)
        SetPlayerStamina(player, stamina)
        -- debug
        debugPrint('SetPlayerStamina called with stamina:', stamina)
    end,
    SetEntityHealth = function(entity, health)
        SetEntityHealth(entity, health)
        -- debug
        debugPrint('SetEntityHealth called with health:', health)
    end,
    SetWeaponRecoilShakeAmplitude = function(weapon, recoilShakeAmplitude)
        SetWeaponRecoilShakeAmplitude(weapon, recoilShakeAmplitude)
        -- debug
        debugPrint('SetWeaponRecoilShakeAmplitude called with weapon:', weapon, 'and recoilShakeAmplitude:', recoilShakeAmplitude)
    end,
    SetFeastDamageMultiplier = function(ped, feastDamageMultiplier)
        SetWeaponDamageModifier('WEAPON_UNARMED', feastDamageMultiplier)
        -- debug
        debugPrint('SetFeastDamageMultiplier called with ped:', ped, 'and feastDamageMultiplier:', feastDamageMultiplier)
    end,
}
```
