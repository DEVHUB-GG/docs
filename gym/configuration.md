---
description: This document describes all configuration options for the DevHub Gym resource.
---

# 🛠️ Configuration

***

## <mark style="color:yellow;">Shared Configuration (</mark><mark style="color:yellow;">`configs/shared.lua`</mark><mark style="color:yellow;">)</mark>

### **Debug Configuration**

Controls debug output for the gym system.

```lua
Shared.Debug = {
    Enabled = true, -- Set to false to disable all debug prints
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

### **DevHub Skill Tree Integration**

Enable or disable integration with the DevHub Skill Tree system.

```lua
Shared.DevhubSkillTreeEnabled = true -- Set to false if you don't want to use devhub_skillTree
```

* **DevhubSkillTreeEnabled**: Set to `true` to use the skill tree system.

***

### **Exercises Configuration**

Defines all available gym stations and their properties.

```lua
Shared.Exercises = {
    {
        uid = "kettlebellswing",
        maxReps = 10, -- Maximum number of reps for an exercise session
        placeOnTheGround = true, -- Place the prop on the ground
        propName = "devhub_gym_kettlebell_rack", -- Prop model name
        propCoords = vec4(...), -- x, y, z, heading
        playerCoords = vec4(...), -- x, y, z, heading (optional, for some exercises)
        dontSpawnProp = true, -- Optional: If true, the prop will not be spawned, but the target will still be active
    },
    -- Add more exercise stations as needed
}
```

* **uid**: Unique identifier for the exercise type.
* **maxReps**: Maximum repetitions per session.
* **placeOnTheGround**: Whether to place the prop on the ground.
* **propName**: The prop model name.
* **propCoords**: World coordinates for the prop.
* **playerCoords**: Where the player stands during the exercise (optional, required for some exercises).
* **dontSpawnProp**: _(Optional)_ If set to `true`, the prop will **not be spawned**, but the **target interaction will remain active**, allowing players to still perform the exercise.

***

## <mark style="color:yellow;">Client Configuration (</mark><mark style="color:yellow;">`configs/client.lua`</mark><mark style="color:yellow;">)</mark>

### **Blip Configuration**

Controls the map blip for the gym.

```lua
Config.Blip = {
    sprite = 311,
    scale = 0.8,
    color = 5,
    name = "Gym",
    coords = vec2(-1204.3674, -1572.0725), -- Blip location
    enabled = true, -- Enable/disable blip
}
```

* **sprite**: Blip icon.
* **scale**: Blip size.
* **color**: Blip color.
* **name**: Blip label.
* **coords**: Blip coordinates.
* **enabled**: Show/hide the blip.

***

### **Force Stop Key**

Defines the key used to interrupt an ongoing exercise manually.

```lua
Config.ForceStopKey = 200 -- Key to force stop exercise (default is 200 = ESC)
```

*   **ForceStopKey**: The key code to cancel an exercise session. Common values include:

    * `200` = ESC
    * `177` = BACKSPACE
    * `73` = X

    Use [FiveM key mapping reference](https://docs.fivem.net/docs/game-references/controls/) to customize.

***

### **Player StateBag**

### **`LocalPlayer.state.gymExercise`**

Indicates whether the player is currently performing a gym activity.

```lua
-- Example usage:
if LocalPlayer.state.gymExercise then
    -- Player is actively exercising
else
    -- Player is idle or not in an exercise session
end
```

* **Type**: `boolean`
* **Default**: `false`
* **Description**:
  * `true` — the player is currently doing an exercise.
  * `false` — the player is not exercising.
* **Usage**: This state can be checked to block certain actions during training or to trigger animations/UI elements.
* **Reference**: [FiveM State Bags Documentation](https://docs.fivem.net/docs/scripting-manual/networking/state-bags/)

***

### **Minigames**

Defines the available minigames and their settings for each difficulty.

```lua
Config.Minigames = {
    ['minigame_1'] = {
        settings = {
            ['easy'] = { boxes = 3, moveSpeed = 2 },
            ['medium'] = { boxes = 5, moveSpeed = 3 },
            ['hard'] = { boxes = 8, moveSpeed = 5 },
        }
    },
    ['minigame_2'] = {
        settings = {
            ['easy'] = { boxes = 3, moveSpeed = 2 },
            ['medium'] = { boxes = 5, moveSpeed = 3 },
            ['hard'] = { boxes = 8, moveSpeed = 5 },
        }
    },
    ['minigame_3'] = {
        settings = {
            ['easy'] = { time = 5, keysToPress = 2 },
            ['medium'] = { time = 10, keysToPress = 4 },
            ['hard'] = { time = 15, keysToPress = 6 },
        }
    },
    ['minigame_4'] = {
        settings = {
            ['easy'] = { zoneWidth = 20, hitSpeed = 0.5, reduceOnHit = 10, regainSpeed = 2, keySequence = {"A", "D"} },
            ['medium'] = { zoneWidth = 20, hitSpeed = 0.75, reduceOnHit = 10, regainSpeed = 3, keySequence = {"A", "D"} },
            ['hard'] = { zoneWidth = 15, hitSpeed = 1, reduceOnHit = 10, regainSpeed = 4, keySequence = {"A", "D"} },
        }
    },
    ['minigame_5'] = {
        settings = {
            ['easy'] = { },
            ['medium'] = { },
            ['hard'] = { },
        }
    },
}
```

* **settings**: Each minigame has settings for `easy`, `medium`, and `hard` difficulties.

***

### **Exercise Types**

Maps each exercise to its minigames and skill tree XP rewards.

```lua
Shared.ExercisesTypes = {
    ["boxing"] = {
        minigames = { 1, 2, 3, 4 }, -- Minigame IDs
        skillTrees = { ["gym"] = 30 }, -- XP for gym skills uid
    },
    -- ...other exercises...
}
```

* **minigames**: List of minigame IDs for the exercise.
* **skillTrees**: XP rewards for each skill tree.

***

### **Prop Spawn Distance**

Controls how far from the player props will spawn.

```lua
Config.PropSpawnDistance = 50.0 -- Distance from player to spawn props (do not exceed 75.0)
```

***

### **Props Menu Offset**

Offsets for the weight selection menu for each prop model.

```lua
Config.PropsMenuOffset = {
    [`devhub_gym_punch_bag`] = vec3(-1.0, 0.0, 1.0),
    [`devhub_gym_kettlebell_rack`] = vec3(0.0, -1.5, 0.0),
    [`devhub_gym_jumping_box`] = vec3(0.0, -1.0, 0.4),
    [`devhub_gym_barbell`] = vec3(1.5, 0.0, 0.4),
    [`devhub_gym_dumbell2_rack`] = vec3(1.0, 0.0, 0.7),
    [`devhub_gym_dumbell1_rack`] = vec3(1.0, 0.0, 0.7),
    [`prop_barbell_02`] = vec3(1.5, 0.0, 0.4),
}
```

* **key**: Prop model hash.
* **value**: Offset vector for menu display.

***

### **Weight Boosts**

Defines XP and difficulty scaling for different weights.

```lua
Config.WeightBoost = {
    ["1"] = { skillUid = false, boost = 1.0, difficulty = "easy" },
    ["5"] = { skillUid = "gym_weights_1", boost = 1.1, difficulty = "easy" },
    ["10"] = { skillUid = "gym_weights_2", boost = 1.2, difficulty = "easy" },
    ["12"] = { skillUid = "gym_weights_3", boost = 1.25, difficulty = "easy" },
    ["15"] = { skillUid = "gym_weights_4", boost = 1.3, difficulty = "easy" },
    ["20"] = { skillUid = "gym_weights_5", boost = 1.35, difficulty = "easy" },
    ["30"] = { skillUid = "gym_weights_6", boost = 1.45, difficulty = "easy" },
    ["40"] = { skillUid = "gym_weights_7", boost = 1.55, difficulty = "easy" },
    ["50"] = { skillUid = "gym_weights_8", boost = 1.6, difficulty = "easy" },
}
```

* **skillUid**: Skill required to use this weight (or `false` for no requirement).
* **boost**: XP multiplier for the weight.
* **difficulty**: Minigame difficulty for the weight.

***

## <mark style="color:yellow;">Server Configuration (</mark><mark style="color:yellow;">`configs/server.lua`</mark><mark style="color:yellow;">)</mark>

### **XP System Integration**

Awards XP to players after completing exercises if the skill tree system is enabled.

<pre class="language-lua"><code class="lang-lua">Config = {}
-- You can add XP logic here if needed, for example:
Config.AddXP = function(source, xp)
   if Shared.DevhubSkillTreeEnabled then
       exports['devhub_skillTree']:addXp('personal', xp, source)
    end
<strong>end
</strong></code></pre>

* **AddXP**: (Optional) Function to add XP to a player, using the skill tree system if enabled.

***

## <mark style="color:yellow;">Exercise Flow</mark>

1. **Player approaches a gym prop** (e.g., kettlebell, punch bag).
2. **Minigame starts** based on exercise type and weight.
3. **Player completes reps**; XP is calculated and awarded.
4. **Skill tree integration** (if enabled) grants XP to the appropriate skill.

***

## <mark style="color:yellow;">Customization</mark>

* Add or remove exercises in `shared.lua`.
* Adjust minigame settings and difficulties in `client.lua`.
* Integrate with other systems by modifying the XP function in `server.lua`.
* Adjust prop spawn distance and menu offsets as needed.

***
