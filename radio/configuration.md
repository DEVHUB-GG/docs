---
description: >-
  This document describes all configuration options for the DevHub Truck robbery
  resource.
---

# 🛠️ Configuration

## <mark style="color:red;">⚠️ REQUIRED STEPS BEFORE USE</mark>

<mark style="color:red;">**1. pma-voice Configuration in server.cfg**</mark>

Make sure that `voice_enableRadioAnim` in `server.cfg` is set to `0`. If it doesn't exist, add to `server.cfg`:

```
setr voice_enableRadioAnim 0
```

<mark style="color:red;">**2. pma-voice Export**</mark>

Add this to the bottom of `pma-voice/client/module/radio.lua`:

```lua
exports("getRadioData", function()
    return radioData
end)
```

***

<mark style="color:yellow;">Shared Configuration (</mark><mark style="color:yellow;">`configs/shared.lua`</mark><mark style="color:yellow;">)</mark>

**Reserved Channels System**

Controls which radio frequencies are restricted to specific jobs or custom conditions.

```lua
Shared.ReservedChannels = {
    { jobs = { "police", "sheriff", "state" }, channels = { 909, 909.01, 909.02, 911 } },
    { jobs = { "police", "sheriff", "state", "ambulance" }, channels = { 912 } },
    { jobs = { "mechanic1" }, channels = { 913 } },
    { jobs = { "mechanic2" }, channels = { 914 } },
    { channels = { 915 }, handler = function(source, channel)
            local crimeJob = "xd"
            if crimeJob == "vagos" then 
                return true
            else 
                return false, "Access only for Vagos members"
            end
        end
    },
}
```

* **jobs**: Array of job names that have access to the specified channels
* **channels**: Array of frequency numbers that are reserved
* **handler**: Optional custom function for advanced access control
  * **Parameters**: `source` (player ID), `channel` (frequency number)
  * **Returns**: `boolean` (access granted), optional `string` (denial message)
* **Usage**: Add new entries to create job-specific radio channels

**Example - Adding a new reserved channel for mechanics:**

```lua
{ jobs = { "mechanic" }, channels = { 920, 920.01, 920.02 } },
```

***

**Antenna Disable System**

Configuration for the antenna hacking/disabling feature.

```lua
Shared.DisableSettings = {
    enabled = true,
    time = 60000 * 20, -- time in ms that antenna is disabled
    item = "dh_antenna_disabler", -- item required to disable antenna or you can set to false to do not require item
}
```

* **enabled**: Enable/disable the antenna disabling feature
* **time**: Duration in milliseconds that antenna remains disabled (default: 20 minutes)
* **item**: Item name required to disable antenna
  * Set to `false` to allow disabling without an item
  * Default: `"dh_antenna_disabler"`

***

**Antenna Debug Mode**

Shows antenna coverage radius on the map for testing purposes.

```lua
Shared.AntennaDebug = false -- set to true to see antenna radius on map
```

* **AntennaDebug**: Set to `true` to visualize antenna coverage areas on the map
* **Usage**: Enable during setup to verify antenna placement and coverage

***

**Antenna Locations**

Defines all radio antenna locations across the map and their signal coverage.

```lua
Shared.AntennaLocations = {
    {prop = "reh_prop_reh_tablet_01a", coords = vector3(-1267.271, -2442.962, 14.603), rotation = vec3(118.426, -87.740, 122.140), radius = 1500.0},
    {prop = "reh_prop_reh_tablet_01a", coords = vector3(1369.801, -2325.915, 61.777), rotation = vec3(118.426, -87.740, 122.140), radius = 1650.0},
    -- ... more locations
}
```

* **prop**: Prop model name for the antenna object
* **coords**: vector3 coordinates (x, y, z) for antenna placement
* **rotation**: vec3 rotation values (pitch, roll, yaw)
* **radius**: Signal coverage radius in game units
* **Default Locations**: 12 antennas strategically placed across the map
* **Coverage Areas**:
  * Los Santos: 1300-1650 unit radius
  * Sandy Shores: 1350-2000 unit radius
  * Paleto Bay: 1200-3000 unit radius

**Adding a New Antenna:**

```lua
{
    prop = "reh_prop_reh_tablet_01a", 
    coords = vector3(x, y, z), 
    rotation = vec3(pitch, roll, yaw), 
    radius = 1500.0
},
```

***

**Radio Item Name**

Defines the inventory item name for the radio.

```lua
Shared.RadioItem = "radio" -- item name for radio, false to disable
```

* **RadioItem**: Item name in your inventory system
* **Default**: `"radio"`
* **Disable**: Set to `false` to allow radio access without an item requirement

***

#### <mark style="color:yellow;">Client Configuration (</mark><mark style="color:yellow;">`configs/client.lua`</mark><mark style="color:yellow;">)</mark>

**Radio Animations**

Defines multiple animation options for holding and using the radio.

```lua
Config.Animations = {
    { -- Animation 1: Holding Radio (RPEmotes style)
        dict = "anim@male@holding_radio",
        anim = "holding_radio_clip",
        flags = 49,
        propModel = "prop_cs_hand_radio",
        bone = 28422,
        offsets = { x = 0.09, y = 0.03, z = 0.0, xRot = -70.0, yRot = 10.0, zRot = 290.0 },
    },
    { -- Animation 2: Reading (Phone style)
        dict = "cellphone@",
        anim = "cellphone_text_read_base",
        flags = 49,
        propModel = "prop_cs_hand_radio",
        bone = 28422,
        offsets = { x = 0.0, y = 0.0, z = 0.0, xRot = 0.0, yRot = 0.0, zRot = 0.0 },
    },
    { -- Animation 3: Generic Radio Chatter
        dict = "random@arrests",
        anim = "generic_radio_chatter",
        flags = 49,
        propModel = false,
        bone = 28422,
        offsets = { x = 0.0, y = 0.0, z = 0.0, xRot = 0.0, yRot = 0.0, zRot = 0.0 },
    },
    { -- Animation 4: Listening
        dict = "cellphone@",
        anim = "cellphone_call_listen_base",
        flags = 49,
        propModel = "prop_cs_hand_radio",
        bone = 28422,
        offsets = { x = 0.0, y = 0.0, z = 0.0, xRot = 0.0, yRot = 0.0, zRot = 0.0 },
    },
}
```

* **dict**: Animation dictionary name
* **anim**: Specific animation name within the dictionary
* **flags**: Animation flags (49 = upperbody + loop)
* **propModel**: Radio prop model (set to `false` for no prop)
* **bone**: Bone ID for prop attachment (28422 = right hand)
* **offsets**: Position and rotation offsets for the prop
  * **x, y, z**: Position offset
  * **xRot, yRot, zRot**: Rotation offset in degrees
* **Players can switch between animations in-game settings**

***

**Aiming Animation**

Special animation used when player is aiming with a weapon while using radio.

```lua
Config.AimingAnim = {
    dict = "arrest",
    anim = "radio_chatter",
    flags = 49,
    propModel = false,
    bone = 28422,
    offsets = { x = 0.0, y = 0.0, z = 0.0, xRot = 0.0, yRot = 0.0, zRot = 0.0 },
}
```

* **Same structure as regular animations**
* **No prop model by default** for compatibility with weapon aiming
* **Automatically activates when player aims while radio is open**

***

#### <mark style="color:yellow;">Translation Configuration (</mark><mark style="color:yellow;">`configs/translation.lua`</mark><mark style="color:yellow;">)</mark>

Customize all user-facing text in the resource. Full multi-language support.

**Tuning Minigame Translations**

```lua
Shared.Lang = {
    ['tuning_signal_tuning'] = "SIGNAL TUNING",
    ['tuning_description'] = "STABILIZE THE DISTORTED FREQUENCY SIGNAL",
    ['tuning_instructions'] = "Match your signal to the ghost reference line...",
    -- ... more tuning translations
}
```

***

#### <mark style="color:yellow;">Items Configuration (</mark><mark style="color:yellow;">`items/items.lua`</mark><mark style="color:yellow;">)</mark>

Add these items to your inventory system:

**Radio Item**

```lua
-- ESX Example
['radio'] = {
    label = 'Radio',
    weight = 1,
    stack = false,
    close = true,
    description = 'A portable radio communication device'
}
```

**Antenna Disabler Item**

```lua
-- ESX Example
['dh_antenna_disabler'] = {
    label = 'Antenna Disabler',
    weight = 1,
    stack = true,
    close = true,
    description = 'Device to temporarily disable radio antennas'
}
```

**Item Images:**

* `items/dh_antenna_disabler.png` - Provided in the resource
* Add to your inventory images folder

***

#### <mark style="color:yellow;">Customization Examples</mark>

**Creating Job-Specific Channels**

Add multiple job groups with shared access:

```lua
Shared.ReservedChannels = {
    -- Emergency Services (Police + EMS)
    { jobs = { "police", "sheriff", "ambulance" }, channels = { 911, 911.01, 911.02 } },
    
    -- Police Only
    { jobs = { "police", "sheriff", "state" }, channels = { 909, 909.1, 909.2 } },
    
    -- Medical Only
    { jobs = { "ambulance", "doctor" }, channels = { 912 } },
    
    -- Mechanics
    { jobs = { "mechanic" }, channels = { 913, 913.1 } },
    
    -- Government
    { jobs = { "government", "mayor" }, channels = { 920 } },
}
```

***

**Custom Access Handler Example**

Create advanced access control based on custom logic:

```lua
{ 
    channels = { 915, 916, 917 }, 
    handler = function(source, channel)
        local xPlayer = ESX.GetPlayerFromId(source)
        
        -- Check gang affiliation
        local gang = xPlayer.getGang() -- Your gang system
        
        if gang == "vagos" and channel == 915 then
            return true
        elseif gang == "ballas" and channel == 916 then
            return true
        elseif gang == "families" and channel == 917 then
            return true
        else
            return false, "This channel is restricted to gang members"
        end
    end
},
```

***

**Making Radio Accessible Without Item**

Allow all players to use radio without needing the item:

```lua
Shared.RadioItem = false  -- Disable item requirement
```

***

#### <mark style="color:yellow;">Advanced Configuration</mark>

**Frequency Range**

The radio supports frequencies from **0.00 to 9999.99 MHz**.

***

**Signal Strength System**

Signal strength is calculated based on:

1. **Distance from nearest antenna**
2. **Antenna radius configuration**
3. **Whether antenna is disabled**

**Signal Levels:**

* **5 bars**: Within optimal range (< 50% of radius)
* **3-4 bars**: Medium range (50-75% of radius)
* **1-2 bars**: Weak signal (75-100% of radius)
* **0 bars**: No signal (outside radius or antenna disabled)

***

**UI Customization**

Players can customize their radio interface:

* **Wallpaper**: None or Custom
* **Background Opacity**: Adjustable transparency
* **Radio Volume**: Individual volume control
* **Button Click Volume**: UI sound control
* **HUD Display**: Toggle on/off
* **Radio Scale**: Size adjustment
* **HUD Scale**: HUD size adjustment
* **Position**: Drag and reposition UI elements
* **Animation**: Choose from available animations

***

#### <mark style="color:yellow;">Troubleshooting</mark>

**Radio not opening:**

* Verify player has the radio item (if `Shared.RadioItem` is set)
* Check F8 console for Lua errors
* Check if resource is started: `/ensure devhub_radio`

**No signal everywhere:**

* Enable `Shared.AntennaDebug = true` to visualize coverage
* Verify antenna coordinates are valid
* Ensure antenna props are spawning correctly

**Reserved channels not working:**

* Verify job names match your framework's job names exactly
* Check player's job with `/job` or similar command
* Review server console for access denial messages
* Test custom handler functions with debug prints

**Antenna hacking not working:**

* Verify player has `dh_antenna_disabler` item
* Check if `Shared.DisableSettings.enabled` is `true`
* Ensure minigame scripts are loaded
* Look for errors during antenna interaction

***
