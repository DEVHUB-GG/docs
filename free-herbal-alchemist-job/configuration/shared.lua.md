---
description: Documentation for shared.lua Configuration
---

# shared.lua

## <mark style="color:yellow;">**General Configuration**</mark>

### **`Shared.CalculateNeedXP(level)`**

Defines the XP required to reach a specific level using a scaling formula.

* Formula: `100 * (level ^ 1.5)` (rounded down to the nearest integer).

**Example:**

```lua
Shared.CalculateNeedXP = function(level)
    return math.floor(100 * (level ^ 1.5))
end
-- Level 5: Shared.CalculateNeedXP(5) returns 223
```

***

### **`Shared.Economy`**

Controls economy-related settings.

* `xp`: Base XP reward per task.
* `money`: Base monetary reward per task.
* `multiplierPerLevel`: Percentage increase in rewards per player level.

**Example:**

```lua
Shared.Economy = {
    xp = 100,
    money = 250,
    multiplierPerLevel = 0.01, -- 1% increase per level
}
-- At level 10, rewards are XP: 110, Money: 275
```

***

### `Sound Volume`

Adjust in game sound

```lua
Shared.SoundVolume = 0.25
```

***

## <mark style="color:yellow;">**Herb Collection**</mark>

### **`Shared.HerbsTypePerTask`**

* `0`: Determines the types of herbs required per task.
  * If `0`, the script uses half of all available herb types.
  * Any other number specifies the exact number of types per task.

**Example:**

```lua
Shared.HerbsTypePerTask = 2 -- Each task will require 2 herb types
```

***

### **`Shared.Herbs`**

Defines each herb's properties, location, and collection behavior.

**Structure:**

* `name`: Internal name of the herb.
* `label`: Display name of the herb.
* `Blip`: Map marker settings.
  * `sprite`: Icon used for the map blip.
  * `color`: Color of the blip.
  * `scale`: Size of the blip.
  * `coords`: Location of the herb spawn area.
* `amountOnMap`: Maximum number of herb instances on the map.
* `chanceToDrop`: Percentage chance of a successful herb collection.
* `maxInQuest`: Maximum number of this herb type required in a quest.
* `props`: List of prop models used to represent the herb in the game world.
* `coords`: Array of locations where the herb spawns.

**Example:**

```lua
Shared.Herbs = {
    {
        name = "rose",
        label = "Rose",
        Blip = {
            sprite = 140,
            color = 5,
            scale = 0.5,
            coords = vec3(400.8, 752.0, 189.0),
        },
        amountOnMap = 40,
        chanceToDrop = 75,
        maxInQuest = 1,
        props = { 'dh_plant_rose' },
        coords = {
            vec3(400.8, 752.0, 189.0),
            vec3(396.7, 756.3, 187.9),
        },
    },
}
```

***

## <mark style="color:yellow;">**Skill Tree Integration**</mark>

### **`Shared.SkillTreeEnabled`**

* `true`: Enables the skill tree system (requires `devhub_skillTree`).
* `false`: Disables skill tree functionality.

**Example:**

```lua
Shared.SkillTreeEnabled = true
```

***

## <mark style="color:yellow;">**Quest System**</mark>

### **`Shared.QuestsEnabled`**

* `true`: Enables quests for the Herbal Alchemist job.
* `false`: Disables quests.

**Example:**

```lua
Shared.QuestsEnabled = true
Shared.Quests = {
    {
        uid = "rose",
        description = "Collect Roses",
        value = 2,
        reward = {
            xp = 1000,
            money = 1000,
        },
        rewardMultiplerPerLevel = 0.1,
        questMultiplerPerLevel = 0.8,
    },
}
```

***

## <mark style="color:yellow;">**Language Strings**</mark>

### **`Shared.Lang`**

Defines the text displayed to players in various parts of the script.

**Example:**

```lua
Shared.Lang = {
    ['JobName'] = "Herbal Alchemist",
    ['JobDescription'] = "Your task is to pick specific flowers marked on the map, brew potions, and sell them.",
    ['Rewards'] = "Rewards",
}
```
