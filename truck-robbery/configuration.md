---
description: >-
  This document describes all configuration options for the DevHub Truck robbery
  resource.
---

# 🛠️ Configuration

***

### <mark style="color:yellow;">Shared Configuration (</mark><mark style="color:yellow;">`configs/shared.lua`</mark><mark style="color:yellow;">)</mark>

#### **Debug Configuration**

Controls debug output for the truck robbery system.

```lua
Shared.Debug = {
    Enabled = true,    -- Set to false to disable all debug prints
    Levels = {
        Info = true,    -- General information
        Success = true, -- Success operations
        Warning = true  -- Warning and potential issues
    }
}
```

* **Enabled**: Enables/disables all debug prints.
* **Levels**: Controls specific debug message types.
  * **Info**: General information about system operations
  * **Success**: Successful operation confirmations
  * **Warning**: Warning messages and potential issues

***

### <mark style="color:yellow;">Client Configuration (</mark><mark style="color:yellow;">`configs/client.lua`</mark><mark style="color:yellow;">)</mark>

#### **Truck Spawn Locations**

Defines where mission trucks will spawn on the map.

```lua
Config.TruckSpawnCoords = {
    vec4(-271.2736, 6069.4941, 31.0700, 124.1105),
}
```

* **TruckSpawnCoords**: Array of vec4 coordinates (x, y, z, heading) where trucks can spawn.
* **Format**: `vec4(x, y, z, heading)`
* **Usage**: Add more coordinates to create multiple possible spawn points for variety.

***

#### **Destination Coordinates**

Defines where players must deliver the stolen truck.

```lua
Config.DesitnationCoords = {
    vec4(1576.2224, 6449.6060, 24.8902, 274.0559)
}
```

* **DesitnationCoords**: Array of vec4 coordinates for delivery locations.
* **Format**: `vec4(x, y, z, heading)`
* **Note**: Multiple destinations can be added for variety.

***

#### **Ped Models**

Defines the peds models that can be used for NPCs in the mission.

```lua
Config.PedModels = {
    "mp_m_freemode_01",
    "mp_f_freemode_01",
}
```

* **PedModels**: Array of ped model names to use for mission NPCs.
* **Default Models**:
  * `mp_m_freemode_01`: Male freemode ped
  * `mp_f_freemode_01`: Female freemode ped
* **Customization**: Add more ped models to increase NPC variety.

***

### <mark style="color:yellow;">Server Configuration (</mark><mark style="color:yellow;">`configs/server.lua`</mark><mark style="color:yellow;">)</mark>

#### **Alarm Triggering Function**

Custom function to handle alarm triggering during the robbery.

```lua
function TriggerAlarm(coords)
    -- TODO Implement alarm triggering logic
    -- Example: Notify police, trigger dispatch, etc.
end
```

* **coords**: The coordinates where the alarm was triggered.
* **Usage**: Customize this function to integrate with your police/dispatch system.
* **Implementation Examples**:
  * Send notification to police players
  * Create a blip on the map
  * Trigger a dispatch call
  * Send alert to specific job roles

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
* **Example**: For Polish translation, replace English text with Polish equivalents.

***

### <mark style="color:yellow;">Customization</mark>

#### **Adding More Spawn Points**

Add additional truck spawn locations for variety:

```lua
Config.TruckSpawnCoords = {
    vec4(-271.2736, 6069.4941, 31.0700, 124.1105),
    vec4(-500.1234, 5800.5678, 32.1500, 90.0000),  -- New spawn point
    vec4(-150.9876, 6200.3456, 30.5000, 180.0000), -- Another spawn point
}
```

***

#### **Adding More Delivery Locations**

Create multiple delivery destinations:

```lua
Config.DesitnationCoords = {
    vec4(1576.2224, 6449.6060, 24.8902, 274.0559),
    vec4(1200.5678, 6300.1234, 25.5000, 180.0000), -- New destination
}
```

***

#### **Customizing NPC Appearance**

Add more diverse ped models:

```lua
Config.PedModels = {
    "mp_m_freemode_01",
    "mp_f_freemode_01",
    "s_m_m_armoured_01",      -- Security guard
    "s_m_m_autoshop_01",      -- Mechanic
    "a_m_m_business_01",      -- Business person
}
```

***

#### **Implementing Custom Alarm System**

Example implementation for the `TriggerAlarm()` function:

```lua
function TriggerAlarm(coords)
    -- Notify all online police officers
    local xPlayers = ESX.GetExtendedPlayers('job', 'police')
    
    for _, xPlayer in pairs(xPlayers) do
        xPlayer.showNotification('Truck robbery in progress!')
        
        -- Create a blip on the map
        TriggerClientEvent('devhub_truckRobbery:createAlarmBlip', xPlayer.source, coords)
    end
    
    -- Send to dispatch system (example)
    -- TriggerEvent('your_dispatch:sendAlert', {
    --     coords = coords,
    --     type = 'truck_robbery',
    --     message = 'Truck robbery in progress'
    -- })
end
```

***

### <mark style="color:yellow;">Troubleshooting</mark>

**Truck not spawning:**

* Verify `Config.TruckSpawnCoords` contains valid coordinates
* Check that the spawn area is clear of obstructions
* Enable debug mode to see spawn-related messages
* Ensure truck model is properly loaded

**Delivery location not working:**

* Check `Config.DesitnationCoords` coordinates are accessible
* Verify the delivery zone trigger radius
* Test coordinates in-game to ensure they're valid

**NPCs not appearing:**

* Verify ped models in `Config.PedModels` are valid
* Ensure ped models are streamed properly
* Try using default GTA V ped models first

**Alarms not triggering:**

* Implement the `TriggerAlarm()` function in `server.lua`
* Verify your dispatch/police notification system is active
* Check server console for Lua errors
* Test the function with debug prints

**Debug Mode:** Enable comprehensive debugging:

```lua
Shared.Debug = {
    Enabled = true,
    Levels = {
        Info = true,
        Success = true,
        Warning = true
    }
}
```

***
