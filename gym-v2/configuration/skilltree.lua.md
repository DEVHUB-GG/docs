---
description: Documentation for skillTree.lua Configuration
---

# skillTree.lua

Connects Gym V2 to [devhub\_skillTree](https://store.devhub.gg/product/6438994). The script runs perfectly fine without it: leave the master switch off if you do not own the skill tree.

{% hint style="info" %}
The master switch is not in this file. It is `Shared.DevhubSkillTreeEnabled` in [shared.lua](shared.lua.md).
{% endhint %}

## <mark style="color:yellow;">**Skills Category**</mark>

```lua
Shared.SkillsCategory = {
    skill = 'gym2',
    title = 'Gym',
}
```

* **skill**: The category key registered in the skill tree. Keep it unique across your server.
* **title**: The category name players see in the skill tree UI.

***

## <mark style="color:yellow;">**Skills List**</mark>

```lua
Shared.SkillsList = {
    ["gym2"] = {
        {
            icon = "fa-solid fa-fire",
            img = "https://upload.devhub.gg/dh_upload/skilltree/beast_mode.webp",
            points = 1,
            uid = "gym_doublerep_3",
            index = 8,
            title = "Beast Mode",
            description = [[6% chance for a rep to count twice.]],
            effect = 6,
            lines = { bottom = false },
        },
        ...
    },
}
```

The node grid of the gym tree. Each entry is one node:

| Field | Meaning |
| --- | --- |
| **uid** | The node's identity. This is what ties it to an effect, see below. |
| **title** | Node name in the tree. |
| **description** | Node description. HTML is allowed. |
| **effect** | The node's value, in percent. What it means depends on the effect it maps to. |
| **points** | Skill points needed to unlock it. |
| **index** | The node's slot in the grid. This is what positions it. |
| **icon** | FontAwesome class. |
| **img** | Image or GIF shown on the node tooltip. |
| **lines** | Which connector lines are drawn from this slot. |

Entries with only `index` and `lines` are pure connectors: empty grid slots that just draw a line between two real nodes.

{% hint style="warning" %}
Changing `index` moves nodes around and rearranges the connector lines. If you only want to retune the tree, edit `effect` and `points` and leave the layout alone.
{% endhint %}

***

## <mark style="color:yellow;">**Gym Skill Effects**</mark>

```lua
Shared.GymSkillEffects = {
    gym_fatigue_1   = "fatigueReduction",
    gym_fatigue_2   = "fatigueReduction",
    gym_fatigue_3   = "fatigueReduction",
    gym_autopass_1  = "minigameChance",
    gym_autopass_2  = "minigameChance",
    gym_skillxp_1   = "skilltreeXp",
    gym_skillxp_2   = "skilltreeXp",
    gym_statxp_1    = "statXp",
    gym_recovery_1  = "recovery",
    gym_recovery_2  = "recovery",
    gym_doublerep_1 = "doubleRep",
    gym_doublerep_2 = "doubleRep",
    gym_doublerep_3 = "doubleRep",
}
```

* **Description**: Maps each gym node's `uid` to the thing it actually boosts. A node whose uid is not in this table is cosmetic as far as the gym is concerned.

| Effect | What it does |
| --- | --- |
| `fatigueReduction` | Fatigue builds up slower. |
| `minigameChance` | Chance a rep's minigame auto-passes. |
| `skilltreeXp` | More Gym skill tree XP from exercises. |
| `statXp` | More Strength and Agility XP per rep. |
| `recovery` | Muscle fatigue recovers faster. |
| `doubleRep` | Chance a rep counts twice. |

Nodes that share an effect **stack**. With all three `doubleRep` tiers unlocked, a player runs at 2 + 4 + 6 = 12% chance for a rep to count twice.

***

## <mark style="color:yellow;">**The generic nodes**</mark>

The gym tree also carries nodes that belong to the skill tree itself rather than to the gym: `moreMaxHp`, `moreStamina`, `moreStaminaRegen`, `runFaster`, `swimFaster`, `hpRegeneration`, `meleeDamageMultiplier`, `feastDamageMultiplier`, `timeUnderwater` and `ignoreTazer`. They are handled by `devhub_skillTree` and need nothing from the gym.

{% hint style="warning" %}
The best skill tree on FiveM, get it [here](https://store.devhub.gg/product/6438994)
{% endhint %}
