---
description: >-
  This document describes all configuration options for the DevHub Truck robbery
  resource.
---

# 🛠️ Configuration

## ⚙️ Configuration

### <mark style="color:yellow;">Shared Configuration (configs/shared.lua)</mark>

#### Development Mode

```lua
Shared.DevelopmentMode = false
```

* **DevelopmentMode**: Enables development/debug features. The `/craftingadmin` command does **not** require this to be enabled — it is available to admins at all times.

***

## 🛡️ Admin Command — `/craftingadmin`

{% hint style="info" %}
**Admin-only command.** This command is available to all players with admin permissions. No additional configuration (such as `DevelopmentMode`) is required.
{% endhint %}

### What is `/craftingadmin`?

The `/craftingadmin` command opens the **in-game Admin Panel,** a powerful, real-time configuration editor that lets server administrators manage the entire crafting system without touching any configuration files.

### Step-by-Step Usage

1. Join the server as an admin
2. Open chat and run `/craftingadmin`
3. The Admin Panel UI will open, make your changes live
4. **Save** the configuration and restart script!

### Permissions

The command checks the player's permission level through `devhub_lib`. Only players recognised as **admins** by the configured framework (ESX group `admin`/`superadmin`, QBCore `god`/`admin`, etc.) can execute `/craftingadmin`. All other players will receive an access-denied notification.

{% hint style="info" %}
Permission handling is managed by `devhub_lib`. Refer to the [Admin System documentation](../scripts/devhub_lib/admin.md) to configure admin groups for your framework.
{% endhint %}

***

#### Categories Configuration

```lua
Shared.Categories = {
    ["all"] = {
        label = "All",
        icon = "fa-solid fa-layer-group",
        order = 0
    },
    ["weapons"] = {
        label = "Weapons",
        icon = "fa-solid fa-gun",
        order = 1
    },
    ["tools"] = {
        label = "Tools",
        icon = "fa-solid fa-wrench",
        order = 2
    },
}
```

* **label**: Display name shown in the UI dropdown
* **icon**: FontAwesome icon class for the category
* **order**: Sort order in the category list (lower = first)

***

#### Blueprints Configuration

```lua
Shared.Blueprints = {
    ["devhub_pistol"] = {
        label = "Pistol Blueprint",
        description = "Basic sidearm blueprint for close-range combat.",
        img = "https://example.com/pistol_bp.webp",
        rarity = "common",
        hidden = false,
        items = {
            { name = "devhub_metal_scrap", amount = 3 },
            { name = "devhub_steel_plate", amount = 2 },
        },
        customUnlock = {
            {
                label = "Complete the 'Rookie Training' mission",
                icon = "fa-solid fa-user-graduate",
                handler = function(source)
                    -- Custom logic to check unlock condition
                    return true -- Return true if condition met
                end
            },
        }
    },
}
```

| Property       | Type    | Description                                                    |
| -------------- | ------- | -------------------------------------------------------------- |
| `label`        | string  | Display name of the blueprint                                  |
| `description`  | string  | Description text shown in tooltip                              |
| `img`          | string  | URL to blueprint image                                         |
| `rarity`       | string  | Rarity tier: `common`, `uncommon`, `rare`, `epic`, `legendary` |
| `hidden`       | boolean | If true, shows as "???" until unlocked                         |
| `items`        | table   | Array of required items to craft the blueprint                 |
| `customUnlock` | table   | Optional custom conditions for unlocking                       |

***

#### Crafts Configuration

```lua
Shared.Crafts = {
    ["weapons_crafts"] = {
        {
            uid = "pistol_standard_01",
            ingredients = {
                { name = "devhub_steel_plate", amount = 3 },
                { name = "devhub_spring_mechanism", amount = 2 },
            },
            maxQuantity = -1,        -- -1 = unlimited
            time = 15,               -- Crafting time in seconds
            result = {
                name = "weapon_pistol",
                amount = 1,
            },
            blueprint = "devhub_pistol",  -- Optional: requires this blueprint
            category = "weapons",
            prop = "w_pi_pistol",         -- Optional: 3D preview prop
            rarity = "common",
        },
    },
}
```

| Property      | Type   | Description                                      |
| ------------- | ------ | ------------------------------------------------ |
| `uid`         | string | Unique identifier for the recipe                 |
| `ingredients` | table  | Array of required materials with name and amount |
| `maxQuantity` | number | Maximum craftable at once (-1 = unlimited)       |
| `time`        | number | Crafting duration in seconds                     |
| `result`      | table  | Output item name and amount                      |
| `blueprint`   | string | Optional blueprint requirement                   |
| `category`    | string | Category key for filtering                       |
| `prop`        | string | Optional 3D prop model for preview               |
| `rarity`      | string | Rarity tier for visual styling                   |

***

#### Crafting Stations Configuration

```lua
Shared.Craftings = {
    {
        id = "weapon_crafting",
        label = "Weapon Crafting",
        craftsId = "weapons_crafts",    -- References Shared.Crafts key
        queueSize = {
            amount = 5,
            custom = function(source)
                -- Custom queue size logic (e.g., VIP gets more slots)
                return nil  -- Return nil to use default amount
            end
        },
        coords = vector4(934.45, -1522.09, 30.08, -179.32),
        prop = "gr_prop_gr_bench_02a",
        camera = {
            x = 934.49,
            y = -1520.90,
            z = 31.49,
            lookAtX = 934.56,
            lookAtY = -1525.89,
            lookAtZ = 31.58,
        },
        attachmentsEnabled = true,
        blip = {
            name = "Weapon Crafting",
            sprite = 566,
            size = 0.8,
            color = 5,
        },
    },
}
```

| Property             | Type     | Description                                  |
| -------------------- | -------- | -------------------------------------------- |
| `id`                 | string   | Unique identifier for the station            |
| `label`              | string   | Display name in UI                           |
| `craftsId`           | string   | Key referencing `Shared.Crafts` recipe group |
| `queueSize.amount`   | number   | Default queue slots                          |
| `queueSize.custom`   | function | Optional function for dynamic queue size     |
| `coords`             | vector4  | World position and heading                   |
| `prop`               | string   | 3D prop model name                           |
| `camera`             | table    | Camera position and lookAt coordinates       |
| `attachmentsEnabled` | boolean  | Enable weapon attachment tab at this station |
| `blip`               | table    | Optional map blip configuration              |

***

#### Community Projects Configuration

```lua
Shared.CommunityProjects = {
    {
        id = "community_project_1",
        label = "New Vehicle Fleet",
        description = "Help us add new vehicles to the server!",
        img = "https://example.com/project.webp",
        order = 0,
        items = {
            { name = "devhub_metal_scrap", amount = 1200 },
            { name = "devhub_spring_mechanism", amount = 600 },
            { name = "devhub_electronic_chip", amount = 500 },
        },
    },
}
```

| Property      | Type   | Description                            |
| ------------- | ------ | -------------------------------------- |
| `id`          | string | Unique identifier for the project      |
| `label`       | string | Project display name                   |
| `description` | string | Project description text               |
| `img`         | string | URL to project image                   |
| `order`       | number | Display order (lower = first)          |
| `items`       | table  | Required materials with target amounts |

***

#### Weapon Attachments Configuration

Weapon attachments are configured in individual files under `configs/attachments/`. Each weapon has its own configuration file.

```lua
-- configs/attachments/WEAPON_ASSAULTRIFLE.lua
Shared.WeaponAttachments["WEAPON_ASSAULTRIFLE"] = {
    label = "Assault Rifle",
    itemName = "weapon_assaultrifle",
    attachments = {
        ["suppressor"] = {
            { name = "COMPONENT_AT_AR_SUPP_02", label = "Suppressor", itemName = "attachment_suppressor" },
        },
        ["scope"] = {
            { name = "COMPONENT_AT_SCOPE_MACRO", label = "Macro Scope", itemName = "attachment_scope_macro" },
            { name = "COMPONENT_AT_SCOPE_MEDIUM", label = "Medium Scope", itemName = "attachment_scope_medium" },
        },
        ["clip"] = {
            { name = "COMPONENT_ASSAULTRIFLE_CLIP_01", label = "Default Clip" },
            { name = "COMPONENT_ASSAULTRIFLE_CLIP_02", label = "Extended Clip", itemName = "attachment_extended_clip" },
        },
        ["grip"] = {
            { name = "COMPONENT_AT_AR_AFGRIP", label = "Grip", itemName = "attachment_grip" },
        },
        ["flashlight"] = {
            { name = "COMPONENT_AT_AR_FLSH", label = "Flashlight", itemName = "attachment_flashlight" },
        },
        ["tint"] = {
            { name = "TINT_NORMAL", label = "Black" },
            { name = "TINT_GREEN", label = "Green", itemName = "tint_green" },
            { name = "TINT_GOLD", label = "Gold", itemName = "tint_gold" },
            -- ... more tints
        },
    }
}
```

| Property      | Type   | Description                                                        |
| ------------- | ------ | ------------------------------------------------------------------ |
| `label`       | string | Weapon display name                                                |
| `itemName`    | string | Inventory item name for the weapon                                 |
| `attachments` | table  | Categories of available attachments                                |
| `name`        | string | GTA component/tint name                                            |
| `label`       | string | Attachment display name                                            |
| `itemName`    | string | Optional: inventory item required (if omitted, attachment is free) |

***

### <mark style="color:yellow;">Client Configuration (configs/client.lua)</mark>

#### UI Settings

```lua
Config.ClickSoundVolume = 0.3
```

* **ClickSoundVolume**: Volume level for UI click sounds (0.0 to 1.0)

#### Blueprint Display

```lua
Config.HideLockedHiddenBlueprints = false
```

* `true` = Items requiring hidden blueprints are completely hidden until unlocked
* `false` = Items are shown but blueprint displays as "???" until unlocked

#### Feature Toggles

```lua
Config.CommunityProjectsEnabled = true
```

* **CommunityProjectsEnabled**: Enable/disable the Community Projects tab

#### Client Functions (Hooks)

```lua
Config.OpenFunctions = {
    ["CanOpenCrafting"] = function(stationId)
        -- Called before opening crafting menu
        -- Return true to allow, false to deny
        -- Example: check if player is dead, in vehicle, etc.
        return true
    end,
    
    ["OnCraftingOpened"] = function(stationId)
        -- Called when crafting menu opens
    end,
    
    ["OnCraftingClosed"] = function(stationId)
        -- Called when crafting menu closes
    end,
    
    ["OnCraftStart"] = function(stationId, recipeId, amount)
        -- Called when player starts crafting
    end,
}
```

| Function           | Parameters                  | Description                                           |
| ------------------ | --------------------------- | ----------------------------------------------------- |
| `CanOpenCrafting`  | stationId                   | Validate if player can open crafting (return boolean) |
| `OnCraftingOpened` | stationId                   | Hook when crafting UI opens                           |
| `OnCraftingClosed` | stationId                   | Hook when crafting UI closes                          |
| `OnCraftStart`     | stationId, recipeId, amount | Hook when craft begins                                |

***

### <mark style="color:yellow;">Server Configuration (configs/server.lua)</mark>

#### Server Functions (Hooks)

```lua
Config.ServerFunctions = {
    ["OnCraftStart"] = function(source, stationId, recipeId, amount)
        -- Called when player starts crafting (server-side)
    end,
    
    ["CanCraft"] = function(source, stationId, recipeId, amount)
        -- Custom validation before crafting starts
        -- Return true to allow, false to deny
        return true
    end,
    
    ["OnAttachmentsApplied"] = function(source, weaponModel, weaponSlot, attachmentsApplied)
        -- Called when player applies attachments to a weapon
        -- attachmentsApplied = array of {category, component, label}
    end,
    
    ["CanUnlockBlueprint"] = function(source, blueprintId)
        -- Custom validation before unlocking a blueprint
        -- Return true to allow, false to deny
        return true
    end,
    
    ["OnBlueprintUnlocked"] = function(source, blueprintId)
        -- Called when player successfully unlocks a blueprint
    end,
    
    ["OnCraftCompleted"] = function(source, stationId, recipeId, amount, resultItem, resultAmount)
        -- Called when a craft finishes (timer reaches 0), before the player claims it
        -- resultItem = item name, resultAmount = total amount to be given
    end,
    
    ["CanClaimItem"] = function(source, stationId, recipeId, amount, resultItem, resultAmount)
        -- Custom validation before claiming a finished craft
        -- Return true to allow, false to deny
        return true
    end,
    
    ["OnItemClaimed"] = function(source, stationId, recipeId, amount, resultItem, resultAmount)
        -- Called after the player successfully claims a crafted item
    end,
}
```

| Function               | Parameters                                                    | Description                                   |
| ---------------------- | ------------------------------------------------------------- | --------------------------------------------- |
| `OnCraftStart`         | source, stationId, recipeId, amount                           | Server-side hook when crafting starts         |
| `CanCraft`             | source, stationId, recipeId, amount                           | Validate craft permission (return boolean)    |
| `OnAttachmentsApplied` | source, weaponModel, weaponSlot, attachmentsApplied           | Hook when attachments are saved               |
| `CanUnlockBlueprint`   | source, blueprintId                                           | Validate blueprint unlock (return boolean)    |
| `OnBlueprintUnlocked`  | source, blueprintId                                           | Hook when blueprint is unlocked               |
| `OnCraftCompleted`     | source, stationId, recipeId, amount, resultItem, resultAmount | Hook when craft timer finishes (before claim) |
| `CanClaimItem`         | source, stationId, recipeId, amount, resultItem, resultAmount | Validate claim permission (return boolean)    |
| `OnItemClaimed`        | source, stationId, recipeId, amount, resultItem, resultAmount | Hook when player claims crafted item          |

#### Community Project Completion Event

```lua
AddEventHandler("devhub_crafting:server:communityProjectCompleted", function(projectId, projectData)
    -- Triggered when a community project reaches 100%
    -- Use this to give rewards, send announcements, etc.
    
    -- Example: Give reward to all online players
    local players = GetPlayers()
    for _, playerId in ipairs(players) do
        -- exports['your_core']:GiveItem(playerId, 'reward_item', 10)
    end
    
    -- Example: Server-wide announcement
    TriggerClientEvent('chat:addMessage', -1, {
        color = {0, 255, 0},
        args = {"Community", "Project '" .. projectData.label .. "' completed!"}
    })
end)
```

***

### <mark style="color:yellow;">Skill Tree Configuration (configs/skillTree.lua)</mark>

#### Enable Skill Tree

The skill tree integration requires `devhub_skillTree` resource. Enable in `shared.lua`:

```lua
Shared.SkillTreeEnabled = true  -- Add this to shared.lua
```

#### Skill Category

```lua
Shared.SkillsCategory = {
    skill = 'crafting',
    title = 'Crafting'
}
```

* **skill**: Internal identifier for the skill category
* **title**: Display name in skill tree UI

#### Skills List

```lua
Shared.SkillsList = {
    ["crafting"] = {
        {
            uid = "skill_7",
            index = 7,
            title = "Fast Crafting",
            icon = "fas fa-bolt",
            img = "https://upload.devhub.gg/dh_upload/crafting/crafting.png",
            description = "Reduces crafting time by 5%.",
            points = 1,
            effect = 5,
            lines = { rightBottom = false },
        },
        {
            uid = "skill_28",
            index = 28,
            title = "Double Craft",
            icon = "fas fa-clone",
            img = "https://upload.devhub.gg/dh_upload/crafting/crafting.png",
            description = "5% chance to receive double crafted items.",
            points = 1,
            effect = 5,
            lines = { bottom = false, rightTop = false, top = false, left = false },
        },
        -- ... more skills
    }
}
```

| Property      | Type    | Description                                         |
| ------------- | ------- | --------------------------------------------------- |
| `uid`         | string  | Unique skill identifier                             |
| `index`       | number  | Grid position in skill tree                         |
| `title`       | string  | Skill display name                                  |
| `icon`        | string  | FontAwesome icon class                              |
| `img`         | string  | Skill icon image URL                                |
| `description` | string  | Skill description text                              |
| `points`      | number  | Skill points required to unlock                     |
| `effect`      | number  | Effect value (e.g., percentage)                     |
| `lines`       | table   | Connection lines to adjacent skills                 |
| `unLockable`  | boolean | If true, skill cannot be unlocked (decorative node) |

#### Available Skill Types

* **Fast Crafting**: Reduces crafting time by X%
* **Double Craft**: X% chance to receive double items
* **Experience Boost**: Gain X% more XP
* **Rare Drop Chance**: Increases rare item drop by X%
* **Material Preservation**: X% chance to not consume materials
* **Blueprint Access**: Unlocks specific weapon blueprints

***

### <mark style="color:yellow;">Rarity Colors Reference</mark>

```lua
-- Used throughout the script for visual styling
local rarities = {
    ["common"] = "#C0C0C0",      -- Silver
    ["uncommon"] = "#00FF00",    -- Green
    ["rare"] = "#00D4FF",        -- Cyan
    ["epic"] = "#8A5BFF",        -- Purple
    ["legendary"] = "#FFA500"    -- Orange
}
```

***

### Configuration Best Practices

1. **Backup configs**: Always backup configuration files before making changes
2. **Unique IDs**: Ensure all `uid`, `id`, and key names are unique across configs
3. **Test incrementally**: Change one setting at a time and test thoroughly
4. **Balance crafting times**: Set appropriate times based on item value/rarity
5. **Blueprint progression**: Design blueprint unlock paths that encourage gameplay
6. **Queue sizes**: Balance queue slots with server economy (VIP perks, etc.)
7. **Attachment items**: Decide which attachments require items vs. free (default clips)
8. **Community projects**: Set realistic material goals based on player population
9. **Image URLs**: Use reliable image hosting
