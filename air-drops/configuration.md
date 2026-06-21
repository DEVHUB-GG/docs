---
description: >-
  This document describes all configuration options for the DevHub Air Drops
  resource.
---

# 🛠️ Configuration

## 🛠️ Configuration

***

### <mark style="color:yellow;">Shared Configuration (</mark><mark style="color:yellow;">`configs/shared.lua`</mark><mark style="color:yellow;">)</mark>

#### **Debug Configuration**

Controls debug output for the air drops system.

```lua
Shared.Debug = {
    Enabled = true,     -- Set to false to disable all debug prints
    Levels = {
        Info = true,    -- General information
        Success = true, -- Success operations
        Warning = true  -- Warning and potential issues
    }
}
```

* **Enabled**: Enables/disables all debug prints.
* **Levels**: Controls specific debug message types (Info, Success, Warning).

***

#### **Drop Presets Configuration**

Defines different categories of items that can be dropped in airdrops. Each preset contains a list of items with their spawn properties.

```lua
Shared.DropPresets = {
    ["food"] = { 
        { name = "bread", amount = {min = 1, max = 3}, chance = 75 },
        { name = "water", amount = {min = 1, max = 5}, chance = 20 },
        -- { name = "canned_food", amount = {min = 1, max = 3}, chance = 5 }
    },
    ["medical"] = {
        { name = "bandage", amount = {min = 1, max = 5}, chance = 33 },
        { name = "first_aid_kit", amount = {min = 1, max = 2}, chance = 33 },
        { name = "painkiller", amount = {min = 1, max = 10}, chance = 34 }
    },
    ["weapons"] = {
        { name = "weapon_pistol", amount = {min = 1, max = 1}, chance = 25 },
        { name = "weapon_rifle", amount = {min = 1, max = 1}, chance = 25 },
        { name = "ammo_9mm", amount = {min = 1, max = 150}, chance = 50 }
    },
}
```

**Structure Explanation:**

* **name**: The item identifier/name (must match your inventory system).
* **amount**: Min and max quantity range for the item.
  * **min**: Minimum amount to spawn.
  * **max**: Maximum amount to spawn.
* **chance**: Percentage chance (0-100) for the item to spawn.

**Note**: Each preset's chances should ideally sum to 100% for proper distribution.

***

#### **Mission Drop Configuration**

Defines special mission-based airdrops with specific loot tables and requirements.

```lua
Shared.MissionDrop = {
    { 
        name = "food", 
        chance = 50, 
        amount = {min = 1, max = 5}, 
        requiredItem = { 
            ["crate_key"] = 3,
            ["tools"] = 1
        }
    },
    { 
        name = "medical", 
        chance = 30, 
        amount = {min = 1, max = 5}, 
        requiredItem = false 
    },
    { 
        name = "weapons", 
        chance = 20, 
        amount = {min = 1, max = 5}, 
        requiredItem = { 
            ["crate_key"] = 1
        } 
    }
}
```

**Structure Explanation:**

* **name**: The drop preset category (references `Shared.DropPresets`).
* **chance**: Percentage chance for this mission drop type to occur.
* **amount**: Number of items from the preset to drop.
  * **min**: Minimum number of items.
  * **max**: Maximum number of items.
* **requiredItem**: Items required to open the crate (false = no requirements).
  * Key: Item name.
  * Value: Required quantity.

***

### <mark style="color:yellow;">Client Configuration (</mark><mark style="color:yellow;">`configs/client.lua`</mark><mark style="color:yellow;">)</mark>

#### **Guards Settings**

Configures NPC guards that protect the air drop zones.

```lua
Config.GuardsSettings = {
    amount = {
        min = 5,  -- Minimum number of guards to spawn
        max = 15  -- Maximum number of guards to spawn
    },
    modelsPresets = {
        { "s_m_y_armymech_01", "s_m_m_armoured_02", "mp_s_m_armoured_01" },
        { "s_m_y_blackops_01", "s_m_y_blackops_02", "s_m_y_blackops_03" },
        { "s_m_m_highsec_01", "s_m_m_highsec_02", "a_m_y_hasjew_01", "a_m_m_hasjew_01" },
        { "g_m_y_lost_02", "g_m_y_lost_01", "g_m_y_lost_03" },
        { "s_m_m_marine_01", "s_m_y_marine_01", "s_m_m_marine_02", "s_m_y_marine_02", "s_m_y_marine_03" },
    },
    weapons = {
        "WEAPON_PISTOL",
        "WEAPON_ASSAULTRIFLE",
        "WEAPON_SMG",
        "WEAPON_CARBINERIFLE",
        "WEAPON_COMBATPISTOL",
        "WEAPON_MICROSMG",
        "WEAPON_PUMPSHOTGUN",
        "WEAPON_ASSAULTSHOTGUN",
    }
}
```

**Configuration Details:**

* **amount**: Random number of guards spawned per air drop.
  * **min**: Minimum guards (5).
  * **max**: Maximum guards (15).
* **modelsPresets**: Arrays of ped models for guard variety.
  * Each array represents a thematic group (military, blackops, security, gang, marines).
  * System randomly selects one preset per air drop for visual consistency.
* **weapons**: List of weapons guards can be equipped with.
  * Guards are randomly assigned weapons from this list.

***

### <mark style="color:yellow;">Server Configuration (</mark><mark style="color:yellow;">`configs/server.lua`</mark><mark style="color:yellow;">)</mark>

#### **Air Drop Zones**

Defines all possible locations where air drops can occur.

```lua
Config.AirDropZones = {
    { coords = vec3(-1215.0327, -1550.9435, 4.3715), radius = 2.0 },
}
```

* **coords**: vec3 coordinates (x, y, z) - the center point of the air drop zone.
* **radius**: The spawn radius around the coordinates (in meters). The air drop will spawn at a random location within this radius from the center point.

**To add more zones:**

```lua
Config.AirDropZones = {
    { coords = vec3(-1215.0327, -1550.9435, 4.3715), radius = 60.0 },   -- 60m spawn radius
    { coords = vec3(2500.0, 3700.0, 45.0), radius = 45.0 },  -- Sandy Shores - 45m radius
    { coords = vec3(-2000.0, 2500.0, 5.0), radius = 35.5 },  -- Paleto Bay - 35.5m radius
}
```

***

#### **Air Drop Stats**

Controls the behavior of falling air drops.

```lua
Config.AirDropStats = {
    fallTime = {
        min = 2000,  -- Minimum fall time in milliseconds
        max = 5000,  -- Maximum fall time in milliseconds
    }
}
```

* **fallTime**: How long the air drop takes to fall from the sky.
  * **min**: Minimum fall duration (2 seconds).
  * **max**: Maximum fall duration (5 seconds).
* **Note**: Actual fall time is randomized between min and max for each drop.

***

### <mark style="color:yellow;">Translation Configuration (</mark><mark style="color:yellow;">`configs/translation.lua`</mark><mark style="color:yellow;">)</mark>

Customize all user-facing text in the resource.

```lua
Shared.Lang = {
    ["weight_too_heavy"] = "Weight is too heavy for you, you need to unlock the skill first",
}
```

* Modify these strings to change the language or customize messages.
* Add your own language translations by following the same format.

***

### <mark style="color:yellow;">Customization</mark>

#### **Adding New Drop Presets**

Create custom loot categories for different scenarios:

```lua
Shared.DropPresets = {
    -- Existing presets...
    ["rare_items"] = {
        { name = "diamond", amount = {min = 1, max = 3}, chance = 10 },
        { name = "gold_bar", amount = {min = 2, max = 5}, chance = 30 },
        { name = "rare_artifact", amount = {min = 1, max = 1}, chance = 60 }
    },
    ["tools"] = {
        { name = "lockpick", amount = {min = 5, max = 15}, chance = 40 },
        { name = "advanced_lockpick", amount = {min = 1, max = 5}, chance = 30 },
        { name = "drill", amount = {min = 1, max = 2}, chance = 30 }
    },
}
```

***

#### **Adjusting Guard Difficulty**

**Easier (Fewer Guards):**

```lua
Config.GuardsSettings = {
    amount = {
        min = 2,
        max = 5
    },
    -- Keep weapons list minimal
    weapons = {
        "WEAPON_PISTOL",
        "WEAPON_COMBATPISTOL",
    }
}
```

**Harder (More Guards & Better Weapons):**

```lua
Config.GuardsSettings = {
    amount = {
        min = 15,
        max = 25
    },
    weapons = {
        "WEAPON_ASSAULTRIFLE",
        "WEAPON_CARBINERIFLE",
        "WEAPON_SPECIALCARBINE",
        "WEAPON_COMBATMG",
        "WEAPON_ASSAULTSHOTGUN",
    }
}
```

***

#### **Modifying Drop Chances**

Adjust the probability of different loot types:

**High-Value Drops (More Weapons/Medical):**

```lua
Shared.MissionDrop = {
    { name = "food", chance = 10, amount = {min = 1, max = 3}, requiredItem = false },
    { name = "medical", chance = 40, amount = {min = 2, max = 6}, requiredItem = false },
    { name = "weapons", chance = 50, amount = {min = 1, max = 3}, requiredItem = { 
            ["crate_key"] = 1
        } 
    }
}
```

**Balanced Drops:**

```lua
Shared.MissionDrop = {
    { name = "food", chance = 33, amount = {min = 2, max = 5}, requiredItem = false },
    { name = "medical", chance = 33, amount = {min = 2, max = 5}, requiredItem = false },
    { name = "weapons", chance = 34, amount = {min = 1, max = 3}, requiredItem = { 
            ["crate_key"] = 2
        } 
    }
}
```

***

#### **Creating Locked vs Unlocked Crates**

Control which crates require items to open:

**All Crates Open (No Requirements):**

```lua
Shared.MissionDrop = {
    { name = "food", chance = 33, amount = {min = 1, max = 5}, requiredItem = false },
    { name = "medical", chance = 33, amount = {min = 1, max = 5}, requiredItem = false },
    { name = "weapons", chance = 34, amount = {min = 1, max = 5}, requiredItem = false }
}
```

**All Crates Locked:**

```lua
Shared.MissionDrop = {
    { name = "food", chance = 33, amount = {min = 1, max = 5}, requiredItem = { 
            ["crate_key"] = 1
        } 
    },
    { name = "medical", chance = 33, amount = {min = 1, max = 5}, requiredItem = { 
            ["crate_key"] = 1
        } 
    },
    { name = "weapons", chance = 34, amount = {min = 1, max = 5}, requiredItem = { 
            ["crate_key"] = 2,
            ["advanced_tools"] = 1
        } 
    }
}
```

***

### <mark style="color:yellow;">Advanced Configuration</mark>

#### **Creating Themed Guard Presets**

Customize guard appearance for different scenarios:

```lua
Config.GuardsSettings = {
    amount = { min = 5, max = 15 },
    modelsPresets = {
        -- Military Theme
        { "s_m_y_armymech_01", "s_m_m_armoured_02", "s_m_m_marine_01" },
        
        -- Police/SWAT Theme
        { "s_m_y_swat_01", "s_m_y_cop_01", "s_m_m_fibsec_01" },
        
        -- Gang Theme
        { "g_m_y_ballasout_01", "g_m_y_famca_01", "g_m_y_mexgang_01" },
        
        -- Private Security Theme
        { "s_m_m_highsec_01", "s_m_m_highsec_02", "mp_m_securoguard_01" },
    },
    weapons = {
        "WEAPON_ASSAULTRIFLE",
        "WEAPON_CARBINERIFLE",
        "WEAPON_SMG",
    }
}
```

***

#### **Dynamic Fall Time Based on Height**

Adjust fall times for different scenarios:

**Quick Drops (Low Altitude):**

```lua
Config.AirDropStats = {
    fallTime = {
        min = 1000,  -- 1 second
        max = 2000,  -- 2 seconds
    }
}
```

**Realistic Drops (High Altitude):**

```lua
Config.AirDropStats = {
    fallTime = {
        min = 5000,  -- 5 seconds
        max = 10000, -- 10 seconds
    }
}
```

**Cinematic Drops (Very Slow):**

```lua
Config.AirDropStats = {
    fallTime = {
        min = 8000,  -- 8 seconds
        max = 15000, -- 15 seconds
    }
}
```

***

### <mark style="color:yellow;">Troubleshooting</mark>

**Air drops not spawning:**

* ✅ Verify `Config.AirDropZones` contains valid coordinates.
* Check server console for errors during resource start.
* Ensure the triggering event/command is correctly configured.

**Guards not spawning:**

* Check `Config.GuardsSettings.amount` values are greater than 0.
* Verify ped model names in `modelsPresets` are valid GTA V models.
* Enable debug mode to see spawn messages.

**Players can't open crates:**

* Verify players have the required items defined in `Shared.MissionDrop.requiredItem`.
* Check item names match your inventory system exactly (case-sensitive).
* Ensure inventory has space for the dropped items.

**Items not being received:**

* ✅ Confirm items in `Shared.DropPresets` exist in your database/item configuration.
* Check item names match exactly (case-sensitive).
* Verify player inventory has sufficient weight capacity.
* Enable debug mode to see item distribution logs.

**Guards not attacking:**

* Check weapon names in `Config.GuardsSettings.weapons` are valid.
* Verify no conflicting resources affecting NPC behavior.

***
