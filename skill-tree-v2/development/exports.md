# Exports

## <mark style="color:yellow;">Parameter Types</mark>

Common parameters used across exports:

* `categoryUid` (string): Category uid (e.g., 'personal')
* `skillUid` (string): Unique skill identifier (e.g., 'swimming\_1', 'melee\_damage\_1')
* `source` (number): Player server ID (required for server-side)
* `amount` (number): Numeric value for XP/points

***

## <mark style="color:yellow;">Client Side Exports</mark>

### Reload Default Skills

Reloads all skills in the 'personal' category to their default states. Can be useful for some people after ambulance respawn.

```lua
exports['devhub_skillTree']:reloadDefaultSkills()
```

***

### Close Skill  Tree

Programmatically closes the skill tree UI.

```lua
exports['devhub_skillTree']:closeSkillTree()
```

***

### Get Config

Returns current configuration of skills and categories.

```lua
exports['devhub_skillTree']:getConfig()
```

**Returns:**

```lua
{
    SkillsCategory = Config.SkillsCategory,
    SkillsList = Config.SkillsList
}
```

***

### Set Pre Made Xp Earning Status

Enable or disable pre-made ways of earning XP. You can use this to disable XP earning temporarily while a player is engaged in another activity.

_status_ **false -** xp will be disabled

_status_ **true  -** xp will be enabled (default set by skill tree script)

```lua
exports['devhub_skillTree']:setPreMadeXpEarningStatus(status)
```

## <mark style="color:yellow;">Server Side Exports</mark>

### Get Player Level

Gets the player's level in a specific skill category.

```lua
exports['devhub_skillTree']:getPlayerLevel(categoryUid, source)
```

**Parameters:**

* `categoryUid` (string): Skill category name
* `source` (number): Player server ID

**Returns:** Current level (number) or 0 if not found

**Example**

```lua
local level = exports['devhub_skillTree']:getPlayerLevel('personal', source)
print('Player level:', level)
```

***

### Get Player Xp

Gets the player's current XP in a specific skill.

```lua
exports['devhub_skillTree']:getPlayerXp(categoryUid, source)
```

**Parameters:**

* `categoryUid` (string): Skill category name
* `source` (number): Player server ID

**Returns:** Current XP (number) or 0 if not found

**Example**

```lua
local xp = exports['devhub_skillTree']:getPlayerXp('personal', source)
print('Current XP:', xp)
```

***

### Get Player Points

Gets the player's available points for a specific skill.

```lua
exports['devhub_skillTree']:getPlayerPoints(categoryUid, source)
```

**Parameters:**

* `categoryUid` (string): Skill category name
* `source` (number): Player server ID

**Returns:** Available points (number) or 0 if not found

**Example**

```lua
local points = exports['devhub_skillTree']:getPlayerPoints('personal', source)
print('Available points:', points)
```

***

### Get Player Total Xp

Gets the player's total accumulated XP in a specific skill.

```lua
exports['devhub_skillTree']:getPlayerTotalXp(categoryUid, source)
```

**Parameters:**

* `categoryUid` (string): Skill category name
* `source` (number): Player server ID

**Returns:** Total XP (number) or 0 if not found

**Example**

```lua
local totalXp = exports['devhub_skillTree']:getPlayerTotalXp('personal', source)
print('Total XP earned:', totalXp)
```

***

### Get Player Global Stats

Gets the player's global statistics across all skills.

```lua
exports['devhub_skillTree']:getPlayerGlobalStats(source)
```

**Parameters:**

* `source` (number): Player server ID

**Returns:**

```lua
{
    totalXp = number,
    totalLevel = number,
    usedPoints = number,
    unlockedSkills = number
}
```

**Example**

```lua
local stats = exports['devhub_skillTree']:getPlayerGlobalStats(source)
print('Total levels:', stats.totalLevel)
```

***

### Get Unlocked Skills

Gets all skills unlocked by the player.

```lua
exports['devhub_skillTree']:getUnlockedSkills(source)
```

**Parameters:**

* `source` (number): Player server ID

**Returns:** Table of unlocked skills or false if player not found

**Example**

```lua
local unlockedSkills = exports['devhub_skillTree']:getUnlockedSkills(source)
if unlockedSkills then
    for skill, _ in pairs(unlockedSkills) do
        print('Unlocked:', skill.categoryUid, skill.uid)
    end
end
```

***

### Remove Xp

```lua
exports['devhub_skillTree']:removeXp(categoryUid, amount, source)
```

Removes XP from a specific skill category.

{% hint style="danger" %}
&#x20;It works only within current level. It will **never** decrease level and will **never** remove unlocked skills.
{% endhint %}

**Parameters:**

* `categoryUid` (string): Skill category name
* `amount` (number): Amount of XP to remove
* `source` (number): Player server ID

**Example**

```lua
exports['devhub_skillTree']:removeXp('personal', 100, source)
```

***

### Remove Points

```lua
exports['devhub_skillTree']:removePoints(categoryUid, amount, source)
```

Removes skill points from a specific category.

**Parameters:**

* `categoryUid` (string): Skill category name
* `amount` (number): Amount of points to remove
* `source` (number): Player server ID

**Example**

```lua
exports['devhub_skillTree']:removePoints('personal', 1, source)
```

***

### Unlock Skill

```lua
exports['devhub_skillTree']:unlockSkill(categoryUid, skillUid, source)
```

**Parameters:**

* `categoryUid` (string): Skill category name
* skillUid (string): Skill uid
* `source` (number): Player server ID

**Example**

```lua
exports['devhub_skillTree']:unlockSkill('personal', 'run_faster_1', source)
```

{% hint style="danger" %}
This export is disabled by default
{% endhint %}

{% hint style="warning" %}
To use unlockSkill export set Config.TurnOnUnlockSkillExport = true
{% endhint %}

***

## <mark style="color:yellow;">Shared Exports</mark>

These exports work on both client and server side.

### Has Unlocked Skill

Checks if a player has unlocked a specific skill.

```lua
exports['devhub_skillTree']:hasUnlockedSkill(categoryUid, skillUid, source)
```

**Parameters:**

* `categoryUid` (string): Skill category name
* `skillUid` (string): Unique identifier of the skill
* `source` (number): Player server ID (server-side only)

**Returns:** `true` if unlocked, `nil` otherwise

**Example**

```lua
-- Client side example
local hasSkill = exports['devhub_skillTree']:hasUnlockedSkill('personal', 'swimming_1')

-- Server side example
local hasSkill = exports['devhub_skillTree']:hasUnlockedSkill('personal', 'swimming_1', source)
```

***

### Add Xp

Adds XP to a specific skill.

```etlua
exports['devhub_skillTree']:addXp(categoryUid, amount, source)
```

**Parameters:**

* `categoryUid` (string): Skill category name
* `amount` (number): Amount of XP to add
* `source` (number): Player server ID (server-side only)

**Example**

```lua
-- Client side example
exports['devhub_skillTree']:addXp('personal', 100)

-- Server side example
exports['devhub_skillTree']:addXp('personal', 100, source)
```

{% hint style="warning" %}
If **Config.DisableSensitiveClientExports** is set to true, this export will <mark style="color:red;">**NOT**</mark> work client side
{% endhint %}

***

### Add Points

Adds skill points to a specific category.

```lua
exports['devhub_skillTree']:addPoints(categoryUid, amount, source)
```

**Parameters:**

* `categoryUid` (string): Skill category name
* `amount` (number): Amount of points to add
* `source` (number): Player server ID (server-side only)

**Example**

```lua
-- Client side example
exports['devhub_skillTree']:addPoints('personal', 1)

-- Server side example
exports['devhub_skillTree']:addPoints('personal', 1, source)
```

{% hint style="warning" %}
If **Config.DisableSensitiveClientExports** is set to true, this export will <mark style="color:red;">**NOT**</mark> work client side
{% endhint %}

***

### Get Skill Effect

Gets the effect value of a specific skill.

```lua
exports['devhub_skillTree']:getSkillEffect(categoryUid, skillUid)
```

**Parameters:**

* `categoryUid` (string): Skill category name
* `skillUid` (string): Unique identifier of the skill

**Returns:** Effect value (number) or 5 if not found

**Example**

```lua
-- Example: Get skill effect value
local effect = exports['devhub_skillTree']:getSkillEffect('personal', 'strength_1')
print('Skill effect:', effect)
```
