---
description: Documentation for client.lua Configuration
---

# client.lua

Everything the player experiences client-side: locations, the device props, the train, the loot props on the carriages and the carry animation. Long coordinate lists are trimmed below to a few entries — open the file to see the full lists. Never rename the `Config.*` keys; only change their values.

## <mark style="color:yellow;">**Debug**</mark>

```lua
Config.Debug = true
```

* **Description**: Enables client-side debug prints to help you track down issues. Set to `false` on a live server.

***

## <mark style="color:yellow;">**TriggerAlarm (client hook)**</mark>

```lua
function TriggerAlarm(coords)
    -- TODO implement your police call logic here (client side)
end
```

* **Description**: Open-function hook called **client-side** (on the player who tipped off the cops) when a police alert fires. Plug your own client dispatch / alarm logic in here, or handle it server side in `configs/server.lua`. `coords` is where the player was when they alerted the police.

***

## <mark style="color:yellow;">**Hack Monitor Props**</mark>

```lua
Config.MonitorProp = 'm24_1_prop_m41_orbital_screen_01a'
Config.MonitorRenderTarget = 'prop_m41_orbital_screen_01a'

Config.SecondMonitorProp = 'prop_monitor_01b'
Config.SecondMonitorRemove = 'prop_monitor_03b'
Config.SecondMonitorRenderTarget = 'tvscreen'
Config.CodeVerifyDuration = 5
Config.CameraPullback = 0.3
```

* **Description**: The props and screens used for the two hacking stages (the antenna monitor and the access terminal).
* **Fields**:
  * **MonitorProp** / **MonitorRenderTarget**: the prop model and its render-target name for the **antenna** monitor.
  * **SecondMonitorProp** / **SecondMonitorRenderTarget**: the prop model and render-target for the **terminal** (second) monitor.
  * **SecondMonitorRemove**: a nearby prop model removed so the second monitor can be placed cleanly.
  * **CodeVerifyDuration**: seconds for the progress bar after the access code is entered on the second monitor.
  * **CameraPullback**: extra meters to pull the monitor camera back from the screen (larger = farther away).

***

## <mark style="color:yellow;">**VehiclePickupLocations**</mark>

```lua
Config.VehiclePickupLocations = {
    vec4(-345.4493, -1567.4952, 25.2286, 38.1397),
    vec4(-369.9995, -1545.5543, 26.1688, 359.9827),
    -- ... more depots
}
```

* **Description**: Depots where the heist vehicle is collected. When a heist starts one is picked at random: the player gets a waypoint, the vehicle spawns once they are close, and getting in begins the heist. Adjust these to real open spots on your map.

***

## <mark style="color:yellow;">**AntennaLocations**</mark>

```lua
Config.AntennaLocations = {
    {
        coords = vector4(2042.2028, 2948.0161, 47.9760, 311.4505), -- Monitor base position
        camera = vector4(2041.7725, 2947.6289, 47.9959, 311.4505), -- Exact camera position
        lookAt = vector3(2042.2028, 2948.0161, 47.9760),           -- Where camera looks
    },
    -- ... more antennas
}
```

* **Description**: The antenna monitors used for the first hack. One is picked per heist; the player is sent there to breach the rail network.
* **Fields**:
  * **coords**: monitor base position (`vector4`, includes heading).
  * **camera**: exact camera position while hacking.
  * **lookAt**: the point the camera looks at.

***

## <mark style="color:yellow;">**SecondHackLocation**</mark>

```lua
Config.SecondHackLocation = {
    coords = vector4(2634.3513, 2931.1013, 44.6104, 165.6576), -- Monitor base position
    camera = vector4(2634.4739, 2931.6267, 45.0104, 166.0450), -- Camera position
    lookAt = vector3(2634.3513, 2931.1013, 44.8104),           -- Where camera looks
}
```

* **Description**: The access terminal where the player enters the code generated during the antenna hack. Same `coords` / `camera` / `lookAt` shape as an antenna entry.

***

## <mark style="color:yellow;">**InterceptionLocations**</mark>

```lua
Config.InterceptionLocations = {
    vector4(2948.3250, 3659.9480, 54.8831, 340.5862),
    vector4(2631.2708, 5449.2334, 62.0930, 21.4040),
    -- ... more interception points
}
```

* **Description**: The points the player travels to in order to plant the device on the tracks. One is chosen per heist; the train is forced to stop just before it.

***

## <mark style="color:yellow;">**TrainRoutes**</mark>

```lua
Config.TrainRoutes = {
    "Sandy Shores Express",
    "Paleto Bay Freight",
    "Los Santos Central",
    -- ... more route names
}
```

* **Description**: Flavour names for the train route, shown in the UI. One is picked at random per heist.

***

## <mark style="color:yellow;">**Scenarios**</mark>

```lua
Config.Scenarios = {
    {
        name = "c4",
        prop = "w_ex_pe",
        hologramProp = "w_ex_pe",
        timer = 15,
        resultProp = "prop_pile_dirt_04",
        animDict = "amb@world_human_gardener_plant@male@base",
        animName = "base",
    },
    {
        name = "jammer",
        prop = "h4_prop_h4_jammer_01a",
        hologramProp = "h4_prop_h4_jammer_01a",
        timer = 15,
        animDict = "amb@world_human_gardener_plant@male@base",
        animName = "base",
    },
    {
        name = "emp",
        prop = "hei_prop_heist_emp",
        hologramProp = "hei_prop_heist_emp",
        timer = 15,
        triggerDistance = 30.0,
        animDict = "mini@repair",
        animName = "fixing_a_player",
    },
}
```

* **Description**: The device the player plants on the tracks to stop the train. One scenario is chosen per heist (`c4`, `jammer` or `emp`).
* **Fields**:
  * **name**: the scenario identifier (matches the planting prompt / translation).
  * **prop**: the prop placed on the tracks.
  * **hologramProp**: the placement-preview hologram prop.
  * **timer**: seconds the planting action takes.
  * **resultProp**: optional prop spawned as the result (e.g. the C4 explosion debris).
  * **triggerDistance**: optional distance (m) at which the device triggers (used by the EMP).
  * **animDict** / **animName**: the planting animation.

***

## <mark style="color:yellow;">**Train Distances**</mark>

```lua
Config.TrainSpawnDistance = 1200.0
Config.TrainStopDistance = 20.0
Config.CarriageFloorZ = 0.0
```

* **TrainSpawnDistance**: how far away (m) the train spawns before approaching.
* **TrainStopDistance**: how many meters before the interception point the train stops.
* **CarriageFloorZ**: base Z offset used to place props on the carriage floor (above the entity origin).

***

## <mark style="color:yellow;">**TrainPropSlots**</mark>

```lua
Config.TrainPropSlots = {
    { slot = 1,  type = "loot",   model = "ch_prop_ch_crate_empty_01a", x = 0.0,  y = -5.5 },
    { slot = 2,  type = "loot",   model = "sm_prop_smug_crate_s_bones", x = 0.0,  y = -4.0 },
    { slot = 3,  type = "search",                                        x = 0.0,  y = -2.1 },
    -- Slots 8-17: positions fixed, models randomly shuffled from remaining LootProps
    { slot = 8,  type = "loot",   x = -0.7, y = 1.4 },
    -- ... more slots
}
```

* **Description**: The fixed layout of loot positions on a carriage.
* **Fields**:
  * **slot**: the slot index.
  * **type**: `"loot"` = a pick-up-able prop, `"search"` = a searchable crate (random model from `Config.SearchProps`).
  * **model**: optional — slots with a model are fixed; slots without one randomly pick from the remaining `Shared.LootProps` pool.
  * **x** / **y**: the prop's offset on the carriage floor.

***

## <mark style="color:yellow;">**Loot Tuning**</mark>

```lua
Config.MinItemsToDeliver = 5
Config.MaxLootProps = 60
Config.MaxSearchProps = 4
Config.PickupDuration = 1500
Config.SearchDuration = 5000
```

* **MinItemsToDeliver**: minimum items that must be loaded into the vehicle before the drop-off can be completed.
* **MaxLootProps**: maximum pickup-able props spawned on the train.
* **MaxSearchProps**: maximum search-only crate props near the train.
* **PickupDuration**: time (ms) to pick up a loot prop.
* **SearchDuration**: time (ms) to search a crate.

***

## <mark style="color:yellow;">**Cleanup Timers**</mark>

```lua
Config.TrainAutoDeleteTime = 900
Config.VehicleDeleteDelay = 120
```

* **TrainAutoDeleteTime**: seconds after the train stops before the train and its props auto-delete.
* **VehicleDeleteDelay**: seconds the frozen vehicle stays at the drop-off before it is deleted.

***

## <mark style="color:yellow;">**SearchProps**</mark>

```lua
Config.SearchProps = {
    "ba_prop_battle_crate_art_02_bc",
    "sm_prop_smug_crate_l_narc",
    "ex_prop_crate_wlife_bc",
    -- ... more crate models
}
```

* **Description**: The crate models used for searchable crates (`type = "search"` slots). A model is picked at random from this list. The items found inside come from `Shared.SearchLoot`.

***

## <mark style="color:yellow;">**DropOffLocations**</mark>

```lua
Config.DropOffLocations = {
    vec4(2594.1421, 494.2607, 108.4748, 88.7598),
    vec4(2152.9807, -844.8054, 78.3766, 106.4809),
    -- ... more drop-offs
}
```

* **Description**: Where the loaded vehicle is delivered to complete the heist. One is chosen per heist and marked on the player's map.

***

## <mark style="color:yellow;">**Carry Settings**</mark>

```lua
Config.CarryBone = 28422
Config.CarryOffset = vector3(0.0, -0.2, 0.15)
Config.CarryRotation = vector3(6.0, 24.0, -89.0)

Config.CarryOverrides = {
    ["hei_prop_heist_emp"]         = { offset = vector3(0.0, -0.2, 0.15), rotation = vector3(6.0, 24.0, -89.0) },
    ["sm_prop_smug_crate_s_bones"] = { offset = vector3(0.0, -0.4, 0.04), rotation = vector3(30.0, 0.0, 0.0) },
    -- ... one override per loot prop
}
```

* **Description**: How a looted prop is attached to the player during the two-handed box-carry idle. Per-prop overrides fine-tune the fit so each model sits correctly against the chest.
* **Fields**:
  * **CarryBone**: the ped bone the prop attaches to (`28422` = the box-carry chest bone).
  * **CarryOffset** / **CarryRotation**: the default attach offset and rotation.
  * **CarryOverrides**: per-model `{ offset, rotation }` overrides keyed by prop model. Tune these in-game if a prop clips.
