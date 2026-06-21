---
description: >-
  This document describes all configuration options for the DevHub House Robbery
  resource.
---

# 🛠️ Configuration

***

### <mark style="color:yellow;">Shared Configuration (</mark><mark style="color:yellow;">`configs/shared.lua`</mark><mark style="color:yellow;">)</mark>

#### **Debug Configuration**

Controls debug output for the house robbery system.

```lua
Shared.Debug = {
    Enabled = false,    -- Set to true to enable all debug prints
    Levels = {
        Info = true,    -- General information
        Success = true, -- Success operations
        Warning = true  -- Warning and potential issues
    }
}
```

* **Enabled**: Enables/disables all debug prints.
* **Levels**: Controls specific debug message types.

***

#### **House Tiers Configuration**

Defines different house interior templates with their props and entry points.

```lua
Shared.HouseTiers = {
    ["tier1"] = {
        {
            props = House1,
            entry = vec3(918.5439, -933.0507, -90.4959),
        },
        -- Additional tier1 house configurations...
    },
    ["tier2"] = {
        -- Tier2 house configurations...
    },
    ["tier3"] = {
        -- Tier3 house configurations...
    }
}
```

* **tier1/tier2/tier3**: Different house difficulty tiers with varying loot quality and quantity.
* **props**: Reference to the house's lootable prop configuration (House1, House2, etc.).
* **entry**: Vector3 coordinates for the interior entry point.

***

#### **House Props Configuration**

Each house (House1, House2, etc.) contains a list of lootable items:

```lua
House1 = {
    {
        itemName = "dh_hr_tv", 
        coords = vec3(926.965942, -933.610474, -90.365089), 
        rotation = vec3(0.010000, 0.010000, -179.990005), 
        prop = "ex_prop_ex_tv_flat_01"
    },
    -- More items...
}
```

* **itemName**: The item given to the player when the prop is collected.
* **coords**: World coordinates of the prop inside the house interior.
* **rotation**: Rotation of the prop (vec3 or vec4 format).
* **prop**: The prop model name.

***

### <mark style="color:yellow;">Client Configuration (</mark><mark style="color:yellow;">`configs/client.lua`</mark><mark style="color:yellow;">)</mark>

#### **Default Location**

Defines where players are teleported if they're in a house area without permission.

```lua
Config.DefaultLocation = vec3(215.1511, -805.2712, 30.8125)
```

* **DefaultLocation**: Coordinates for the default teleport location (usually outside the restricted area).

***

### <mark style="color:yellow;">Server Configuration (</mark><mark style="color:yellow;">`configs/server.lua`</mark><mark style="color:yellow;">)</mark>

#### **Alarm Triggering Function**

Custom function to handle alarm triggering when sensors are activated.

```lua
function TriggerAlarm(source, coords)
    -- TODO Implement alarm triggering logic
    -- Example: Notify police, trigger dispatch, etc.
end
```

* **source**: The player's server ID who triggered the alarm.
* **coords**: The coordinates where the alarm was triggered.
* **Usage**: Customize this function to integrate with your police/dispatch system.

***

#### **Tier Chance Configuration**

Defines the probability of encountering higher-tier houses.

```lua
Config.TiersChance = {
    ["tier2"] = 50, -- 50% chance for tier2
    ["tier3"] = 25, -- 25% chance for tier3
}
```

* **tier2**: Percentage chance for tier2 houses (50%).
* **tier3**: Percentage chance for tier3 houses (25%).
* **Note**: If neither tier2 nor tier3 is selected, tier1 is used by default (25% remaining chance).

***

#### **House Coordinates**

Defines all available house locations for each tier across the map.

```lua
Config.HousesCoords = {
    ["tier1"] = {
        vec4(118.40, -1921.16, 21.32, 236.39),
        vec4(100.99, -1912.22, 21.41, 333.73),
        -- More coordinates...
    },
    ["tier2"] = {
        vec4(-565.67, 761.20, 185.42, 229.32),
        -- More coordinates...
    },
    ["tier3"] = {
        vec4(-189.04, 1008.74, 232.13, 272.66),
        -- More coordinates...
    },
}
```

* **tier1/tier2/tier3**: Arrays of vec4 coordinates (x, y, z, heading) for house entrances on the map.
* **Usage**: These coordinates mark where players can start a robbery.

***

#### **Cooldown System**

Controls how long players must wait before starting another robbery.

```lua
Config.Cooldown = 30 * 60 -- 30 minutes in seconds
```

* **Cooldown**: Time in seconds before another robbery can be started (default: 30 minutes).

***

#### **Sensor Activation System**

Defines the probability of triggering a sensor based on the number of items collected.

```lua
Config.PropSensorActivationPercent = {
    [1] = 5,    -- 5% chance on 1st item
    [2] = 5,    -- 5% chance on 2nd item
    [3] = 10,   -- 10% chance on 3rd item
    [4] = 10,   -- 10% chance on 4th item
    [5] = 20,   -- 20% chance on 5th item
    [6] = 20,   -- 20% chance on 6th item
    [7] = 25,   -- 25% chance on 7th item
    [8] = 25,   -- 25% chance on 8th item
    [9] = 30,   -- 30% chance on 9th item
    [10] = 30,  -- 30% chance on 10th item
}
```

* **Key**: Number of items collected.
* **Value**: Percentage chance to trigger the alarm sensor.
* **Behavior**: For items beyond 10, the chance remains at 30%.
* **Example**: Collecting the 5th item has a 20% chance to trigger the alarm.

***

### <mark style="color:yellow;">Translation Configuration (</mark><mark style="color:yellow;">`configs/translation.lua`</mark><mark style="color:yellow;">)</mark>

Customize all user-facing text in the resource.

```lua
Shared.Lang = {
    ["weight_too_heavy"] = "Weight is too heavy for you, you need to unlock the skill first",
    ["select_weight"] = "SELECT WEIGHT",
    ["confirm"] = "CONFIRM",
    ["move"] = "MOVE",
    ['kg'] = "KG",
    ["already_exercising"] = "You are already exercising",
    ["station_busy"] = "Station is busy",
    ["start_exercise"] = "Start Exercise",
}
```

* Modify these strings to change the language or customize messages.
* Add your own language translations by following the same format.

***

### <mark style="color:yellow;">Skill Tree Configuration (</mark><mark style="color:yellow;">`configs/skillTree.lua`</mark><mark style="color:yellow;">)</mark>

#### **Enable/Disable Skill Tree**

To enable the skill tree integration, modify `Shared.DevhubSkillTreeEnabled` in `configs/shared.lua`:

```lua
Shared.DevhubSkillTreeEnabled = true -- Set to false if you don't want to use devhub_skillTree
```

***

#### **Skill Category**

Defines the skill tree category for the house robbery system.

```lua
Shared.SkillsCategory = {
    skill = 'gym',  -- Category identifier
    title = 'Gym',  -- Display title
}
```

***

### <mark style="color:yellow;">Robbery Flow</mark>

1. **Player approaches a house entrance** from `Config.HousesCoords`.
2. **Tier is randomly selected** based on `Config.TiersChance`.
3. **Player enters the house interior** (teleported to `entry` coordinates).
4. **Player collects lootable props** (items defined in House1, House2, etc.).
5. **Sensor chance increases** with each item collected (`Config.PropSensorActivationPercent`).
6. **If sensor triggers**, `TriggerAlarm()` function is called.
7. **Player exits the house**

***

### <mark style="color:yellow;">Customization</mark>

* **Add new houses**: Create new house prop configurations (e.g., `House7`) and add entries to `Shared.HouseTiers`.
* **Modify loot tables**: Edit the `itemName` values in each house configuration to change available loot.
* **Adjust difficulty**: Modify `Config.TiersChance` and `Config.PropSensorActivationPercent` to change risk/reward balance.
* **Add more locations**: Expand `Config.HousesCoords` arrays with new vec4 coordinates.
* **Customize alarm behavior**: Implement your logic in the `TriggerAlarm()` function in `server.lua`.
* **Change cooldown**: Modify `Config.Cooldown` to adjust wait time between robberies.
* **Integrate with skill tree**: Enable `Shared.DevhubSkillTreeEnabled` and configure skills in `skillTree.lua`.

***

### <mark style="color:yellow;">Advanced Configuration</mark>

#### **Adding a New House Interior**

1. Create a new house configuration in `configs/shared.lua`:

```lua
House7 = {
    {itemName = "dh_hr_laptop", coords = vec3(x, y, z), rotation = vec3(rx, ry, rz), prop = "prop_laptop_01a"},
    {itemName = "dh_hr_gold_cd", coords = vec3(x, y, z), rotation = vec3(rx, ry, rz), prop = "prop_cd_case_01"},
    -- Add more props...
}
```

2. Add the new house to a tier in `Shared.HouseTiers`:

```lua
["tier1"] = {
    {
        props = House7,
        entry = vec3(x, y, z),
    },
}
```

***

#### **Modifying Sensor Difficulty**

To make robberies easier or harder, adjust the sensor activation percentages:

**Easier (Lower Alarm Chance):**

```lua
Config.PropSensorActivationPercent = {
    [1] = 2,
    [2] = 2,
    [3] = 5,
    [4] = 5,
    [5] = 8,
    -- etc.
}
```

**Harder (Higher Alarm Chance):**

```lua
Config.PropSensorActivationPercent = {
    [1] = 15,
    [2] = 20,
    [3] = 25,
    [4] = 30,
    [5] = 40,
    -- etc.
}
```

***

### <mark style="color:yellow;">Item Configuration</mark>

The resource includes item configuration files for different inventory systems:

* **`items/esx_items.lua`**: ESX framework items
* **`items/qb_items.lua`**: QBCore framework items
* **`items/img/`**: Item images for inventory systems

Ensure the items defined in `itemName` fields match your inventory system's item names.

***

### <mark style="color:yellow;">Required Resources</mark>

#### **⚠️ IMPORTANT: Asset Dependency**

This resource **requires** the `devhub_houseRobbery_assets` resource to function properly.

**Installation Steps:**

1.  Ensure both resources are in your resources folder:

    ```
    resources/
    ├── [devhub]/
    │   ├── devhub_houseRobbery/
    │   └── devhub_houseRobbery_assets/
    ```
2.  Start both resources in your `server.cfg` in the correct order:

    ```cfg
    ensure devhub_houseRobbery_assets
    ensure devhub_houseRobbery
    ```

**Why is this required?**

* `devhub_houseRobbery_assets` contains all prop models, textures, and interior assets.
* Without it, props will not spawn and interiors will not load correctly.

***

### <mark style="color:yellow;">Troubleshooting</mark>

**Resource not starting:**

* ✅ Verify `devhub_houseRobbery_assets` is started **before** `devhub_houseRobbery`.
* Check server console for any asset loading errors.

**Players can't start robberies:**

* Check if `Config.Cooldown` has expired.
* Verify `Config.HousesCoords` contains valid coordinates.
* Ensure players have the required items/permissions (if implemented).
* ✅ Confirm `devhub_houseRobbery_assets` is running.

**Alarms not triggering:**

* Implement the `TriggerAlarm()` function in `server.lua`.
* Check `Config.PropSensorActivationPercent` values are greater than 0.

**Props not spawning:**

* ✅ **First, ensure `devhub_houseRobbery_assets` is started!**
* Verify prop model names in house configurations are valid.
* Check coordinates are within the interior boundaries.
* Enable debug mode to see error messages.
* Restart both resources in the correct order.

***
