---
description: >-
  Welcome to the configuration guide for DevHub 3D Crafting Table! This document
  will walk you through setting up and customizing your crafting experience. All
  primary configuration files are located in
---

# 🛠️ Configuration

## 🛠️ Configuration

#### 📁 Configuration Files Overview:

* `configs/shared.lua`: Settings accessible by both the server and client (e.g., recipe definitions, debug modes).
* `configs/client.lua`: Client-side settings (e.g., UI, camera, sound, developer mode).
* `configs/server.lua`: Server-side settings (e.g., table locations, XP integration).

***

## <mark style="color:yellow;">Shared Configuration (</mark><mark style="color:yellow;">`configs/shared.lua`</mark><mark style="color:yellow;">)</mark>

These settings are shared between the server and the client.

### **Debug Configuration**

Controls whether debug messages are displayed in the console.

```lua
Shared.Debug = {
    Enabled = true, -- Set to false to disable all debug prints
    Levels = {
        Info = true,    -- General information
        Success = true, -- Success operations
        Warning = true  -- Warnings and potential issues
    }
}
```

* **`Enabled`**: `true` to show debug messages, `false` to hide them.
* **`Levels`**: Fine-tune which types of messages you want to see:
  * `Info`: General script information.
  * `Success`: Confirmation of successful actions.
  * `Warning`: Potential issues or misconfigurations.

***

### DevHub Skill Tree Integration

This setting enables integration with the **DevHub Skill Tree System**.

```lua
Shared.DEVHUB_SKILLTREE_ENABLED = false
```

* **DEVHUB\_SKILLTREE\_ENABLED**: Set to `true` if you're using the DevHub Skill Tree system.
* The skill tree system can be purchased at: [DevHub Store](https://store.devhub.gg/product/6440792-1).

***

### Recepie Configuration

This section contains generated code from how-to-create-table.md

```lua
Shared.Recepie = { 
    -- Generated code
}
```

* **Recepie**: Stores generated recipe data.

***

### Multiple Recipes Configuration

The `Shared.MultipleRecipes` allows you to create custom recipes with specific item requirements. This system provides more flexibility for creating multiple recipes under a single crafting table.

```lua
Shared.MultipleRecipes = {
    ["carft1"] = {
        itemsNeeded = {
            {
                name = "items19",
                amount = 1,
            },
            {
                name = "items15",
                amount = 1,
            },
            -- Add more required items
        },
        rewards = {
            {
                name = "items14",
                amount = 1,
            },
            -- Add more reward items
        },
        tableUid = "herbal_table", -- Reference to table type from Shared.Recepie
    },
    ["carft2"] = {
        itemsNeeded = {
            {
                name = "items4",
                amount = 1,
            },
            {
                name = "items9",
                amount = 1,
            },
        },
        rewards = {
            {
                name = "items1",
                amount = 1,
            },
        },
        tableUid = "spices",
    },
    -- Add more custom recipes
}
```

## <mark style="color:yellow;">How to configurate multiple recepies?</mark> [multiple-recepies-configuration.md](multiple-recepies-configuration.md "mention")

## <mark style="color:yellow;">Client Configuration (client.lua)</mark>

### Developer Mode

Enables developer commands, such as `/tablegenerator`.

```lua
Config.DeveloperMode = true 
```

* **DeveloperMode**: Set to `true` to enable developer commands.

***

### Target Interaction Text

Configures the text displayed when targeting a crafting table.

```lua
Config.UseTableTargetText = "Use Table"
```

* **UseTableTargetText**: The text shown when targeting a crafting table for interaction.

***

### Skill Tree Boosts

Defines various skill-based bonuses that apply when using the skill tree integration.

```lua
Config.SkillTreeBoosts = {
    ["table_boost1"] = { 
        attribute = "double_xp",
        chance = 10,
    },
    ["table_boost2"] = {
        attribute = "xp_multiplier",
        chance = 10,
    },
    ["table_boost3"] = {
        attribute = "double_reward",
        chance = 10,
    },
    ["table_boost4"] = {
        attribute = "needed_items_recover",
        chance = 10,
    }
}
```

* **SkillTreeBoosts**: Defines the various skill effects that can be applied during crafting.
  * `double_xp`: 10% chance to earn double XP from crafting.
  * `xp_multiplier`: 10% XP gain multiplier.
  * `double_reward`: 10% chance to receive double rewards.
  * `needed_items_recover`: 10% chance to recover 50% of required crafting items.

***

### Crafting Table Interaction

**Table Check Interval**

Defines how often the system checks if a player is near a crafting table.

```lua
Config.TablesCheckInterval = 2000 
```

* **TablesCheckInterval**: Time in milliseconds between each proximity check (default: `2000ms`).

***

### **Sound Configuration**

Controls the volume of all crafting-related sound effects.

```lua
Config.SoundVolume = 0.5 
```

* **SoundVolume**: The global volume for crafting sounds (`0.0` - `1.0`).

***

### Camera Offsets

Defines camera positions for different crafting table models.

```lua
Config.CameraOffsets = {
    [`dh_alchemy_table`] = {
        cameraAimOffset = vec3(0.3, 0.0, 0.0),
        cameraOffset = vec3(0.3, -2.0, 1.75),
    },
    [`h4_prop_h4_table_isl_01a`] = {
        cameraAimOffset = vec3(0.0, 0.0, 0.0),
        cameraOffset = vec3(0.3, -2.5, 2.6),
    },
    [`bkr_prop_weed_table_01b`] = {
        cameraAimOffset = vec3(0.0, 0.0, 0.0),
        cameraOffset = vec3(0.0, -2.0, 2.2),
    },
}
```

* **CameraOffsets**: Configures camera behavior when interacting with different crafting tables.
  * `cameraAimOffset`: Adjusts where the camera is aiming (relative to the table).
  * `cameraOffset`: Controls the position of the camera relative to the table.

## <mark style="color:yellow;">Server Configuration (server.lua)</mark>

### Crafting Tables Configuration

Defines crafting table locations and settings. This script can be used to spawn crafting tables, or you can spawn them via your own scripts.

```lua
Config.Tables = { 
    { 
        coords = vec4(250.5990, -753.7527, 34.6390, 68.1239), 
        tableUid = "herbal_table", -- Old method: using tableUid
        snapToGround = true 
    },
    { 
        coords = vec4(3629.7241, 5683.1719, 7.8236, 343.4010), 
        recipeUids = { "carft1", "carft3", "carft2", "carft4", "carft5" }, -- New method: using recipeUids
        snapToGround = true 
    },
    -- You can add more tables with different locations and recipes
}
```

**Table Configuration Options:**

* **Tables**: A list of predefined crafting tables.
  * `coords`: The **vector4** position of the table in the world.
  * `snapToGround`: If `true`, the table will be placed on the ground automatically.

**Two Methods for Recipe Assignment:**

1. **Old Method - `tableUid`**:
   * `tableUid`: A unique identifier that matches a key in `Shared.Recepie`.
   * This method links the table to all recipes defined under that specific `tableUid` in the recipe configuration.
2. **New Method - `recipeUids`**:
   * `recipeUids`: An array of recipe UIDs from `Shared.MultipleRecipes`.
   * This method allows you to specify exactly which recipes are available at this table.
   * More flexible as you can mix and match recipes from different categories.

### XP System Integration

This function awards XP to players after crafting if the **DevHub Skill Tree System** is enabled.

```lua
Config.AddXP = function(source, xp)
    -- Add XP to player
    if Shared.DEVHUB_SKILLTREE_ENABLED then 
        -- Server side example
        exports['devhub_skillTree']:addXp('personal', xp, source)
    end
end
```

* **AddXP**: A function that adds XP to the player.
  * Uses `exports['devhub_skillTree']:addXp()` when **DevHub Skill Tree** is enabled.
  * XP is applied to the **personal** skill tree.

## <mark style="color:red;">**Alternative: Triggering the Client-Side Event**</mark>

If you prefer **not to use** `Config.Tables`, you can manually trigger the client-side event instead:

```lua
TriggerEvent("devhub_3dCraftingTable:useTable:client", entity, tableUid)
```

* <mark style="color:red;">**Triggering this event manually allows you to use your own logic for spawning crafting tables.**</mark>
* The event takes two arguments:
  * `entity`: The table entity.
  * `tableUid`: The unique identifier of the table.
* This provides flexibility in integrating the crafting system into custom scripts.

***
