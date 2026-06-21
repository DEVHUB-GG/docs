---
description: Documentation for skillTree.lua Configuration
---

# skillTree.lua

## <mark style="color:yellow;">**XP for Finishing a Job**</mark>

```lua
Shared.XPForFinishedJob = 30
```

* **Description**: Specifies the amount of XP awarded to the skill tree when a player completes a job.
* **Example**: `30` XP is granted upon finishing a job.

***

## <mark style="color:yellow;">**Skills List**</mark>

```lua
Shared.SkillsList = {
    ["miner"] = {
        {
            uid = "cash_boost_4",
            title = "Golden Touch IV",
            description = "Increases cash earned by 5%.",
            effect = 5,
            points = 1,
            index = 9,
        },
        -- Additional skills...
    },
}
```

* **uid**: Unique identifier for the skill.
* **title**: Name of the skill.
* **description**: Explains the effect of the skill.
* **effect**: Value of the skill's effect (e.g., 5% bonus).
* **points**: Points required to unlock the skill.
* **index**: Skill's position in the skill tree.

{% hint style="warning" %}
The best skill tree on fivem, get it [here](https://store.devhub.gg/product/6440792-1)
{% endhint %}

