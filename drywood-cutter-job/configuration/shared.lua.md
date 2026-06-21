---
description: Documentation for shared.lua Configuration
---

# shared.lua

## <mark style="color:yellow;">**XP Calculation Function**</mark>

```lua
Shared.CalculateNeedXP = function(level)
    return math.floor(100 * (level ^ 1.5))
end
```

* **Description**: Determines the XP required for a given level.
* **Formula**: `100 * (level ^ 1.5)`, rounded down.

***

## <mark style="color:yellow;">**Economy Configuration**</mark>

```lua
Shared.Economy = {
    xp = 100,
    money = 250,
    mulitplerPerLevel = 0.01,
}
```

* **xp**: XP earned per finished job.
* **money**: Money earned per finished job.
* **mulitplerPerLevel**: Additional reward percentage per player level.

***

## <mark style="color:yellow;">Job Requirement Configuration</mark>

```lua
Shared.JobRequired = false
```

**Description**: Specifies if a specific job is required to perform mining tasks.\
**Values**: Set to the job name (e.g., "miner") to require it, or `false` to allow all players.

***

## <mark style="color:yellow;">**Skill Tree Integration**</mark>

```lua
Shared.SkillTreeEnabled = false
```

* **Description**: Enables or disables the skill tree feature.
* YOU NEED SKILL TREE SCRIPT, [NORMAL](https://store.devhub.gg/package/6438994) OR [EXCLUSIVE](https://store.devhub.gg/package/6441349)&#x20;
* **Note**: Must ensure the script is loaded after `dh_skillTree`.

***

## <mark style="color:yellow;">**Quest Configuration**</mark>

```lua
Shared.QuestsEnabled = true
Shared.Quests = {
    {
        uid = "wood",
        description = "Collect wood",
        value = 2,
        reward = {
            xp = 1000,
            money = 1000,
        },
        rewardMultiplerPerLevel = 0.1,
        questMultiplerPerLevel = 0.8,
    },
    -- Additional quests...
}
```

* **uid**: Unique identifier for the quest.
* **description**: Describes the quest objective.
* **value**: Number of items or actions required to complete the quest.
* **reward**: XP and money granted upon completion.
* **rewardMultiplerPerLevel**: Multiplier applied to rewards based on player level.
* **questMultiplerPerLevel**: Adjusts the difficulty based on player level.

***

## <mark style="color:yellow;">**Language Configuration**</mark>

```lua
Shared.Lang = {
    ['Your'] = "Your",
    ['Mission'] = "Mission",
    ['Minigame'] = "Minigame",
    ['Time'] = "Time",
    ['JobName'] = "Miner",
    ['JobDescription'] = "As a miner, your job is to extract raw ores from designated mining areas...",
    -- Additional language entries...
}
```

* **Description**: Customizes text used throughout the script's UI.
* **Usage**: Adjust the right-side values to localize or customize messages.
* **Note**: Keep `${value}` format unchanged to ensure dynamic values work correctly.

***

## <mark style="color:yellow;">Sound Volume</mark>

Adjust in game sound

```lua
Shared.SoundVolume = 0.25
```
