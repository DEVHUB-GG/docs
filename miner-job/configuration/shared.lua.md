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

## <mark style="color:yellow;">Extra Rewards Configuration</mark>

The Extra Rewards feature allows players to receive additional items during mining and smelting. To enable or disable this feature, modify the `Shared.ExtraRewardsEnabled` value.

```lua
Shared.ExtraRewardsEnabled = false
```

* **Description**: Enables or disables the extra rewards feature.
* **Values**: Set to `true` to enable or `false` to disable.

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
        uid = "diamond",
        description = "Collect diamonds",
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

## <mark style="color:yellow;">**Ore Configuration**</mark>

```lua
Shared.Ores = {
    {
        props = {
            "dh_miner_ore_diamond1",
            "dh_miner_ore_diamond3",
        },
        item = "diamond",
        label = "Diamond",
        maxInQuest = 3,
        amountOnMap = 5,
        chanceToDrop = 80,
        coords = {
            vec3(2928.759033, 2745.326660, 53.625084),
            -- Additional coordinates...
        },
        extraRewards = {
        {
            itemName = "ore", -- The item the player receives during mining
            mineAmount = 1, -- The amount of items the player receives during mining
            smeltingRewardItem = "ingot", -- The item the player receives during smelting
            smeltingAmount = 1, -- The amount of items the player receives during smelting
            smeltingChance = 50 -- The chance (in %) to receive the item during smelting, item from mining will be removed
        },
    },
    -- Additional ores...
}
```

* **props**: Defines the models used for the ore.
* **item**: Item name corresponding to the collected ore.
* **label**: Display name for the ore.
* **maxInQuest**: Maximum quantity that can be collected during a quest.
* **amountOnMap**: Number of ore entities that spawn on the map.
* **chanceToDrop**: Probability (in percentage) of successfully collecting the ore.
* **coords**: Positions where ores can be found.

### Extra Rewards:

* **itemName**: The item that the player receives during mining.
* **mineAmount**: The number of items received during mining.
* **smeltingRewardItem**: The item that the player receives during smelting.
* **smeltingAmount**: The number of items received during smelting.
* **smeltingChance**: The percentage chance to receive the smelted item, removing the item obtained from mining.

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
