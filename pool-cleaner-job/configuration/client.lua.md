---
description: Documentation for client.lua Configuration
---

# client.lua

## <mark style="color:yellow;">**Main Ped Configuration**</mark>

```lua
Config.MainPed = {
    model = "s_m_y_construct_02",
    coords = vec4(2832.3860, 2798.6382, 57.4497, 100.6738),
    sprite = 527,
    color = 5,
    scale = 0.8,
    name = "Pool Cleaner Job",
}
```

* **Model**: Ped model used for the Miner NPC (`s_m_y_construct_02`).
* **Coordinates**: Position of the NPC on the map.
* **Marker Details**:
  * **Sprite**: `527`
  * **Color**: `5`
  * **Scale**: `0.8`
* **Name**: Displayed as "Pool Cleaner" when interacting.

***

## <mark style="color:yellow;">**Selling Configuration**</mark>

```lua
Config.Selling = {
    coords = vec3(237.9128, -412.8228, 48.1119),
    sprite = 617,
    color = 5,
    scale = 0.8,
    name = "Selling",
}
```

* **Description**: Defines the location and appearance of the selling point.
* **Coordinates**
* **Marker Details**:
  * **Sprite**: `617`
  * **Color**: `5`
  * **Scale**: `0.8`
* **Name**: Displayed as "Selling" when interacting.

***

## <mark style="color:yellow;">**Clothing Configuration**</mark>

```lua
Config.Clothing = {
    ["mp_m_freemode_01"] = { 
        {
            img = "https://cfx-nui-dh_poolCleaner/html/host/jobCloth_m1.png",
            clothes = {
                torso = {65, 3},
                tshirt = { 155, 0 },
                pants = { 38, 3 },
                boots = { 27, 0 },
                gloves = { 64, 0 },
                hat = { 144, 3 },
            }
        },
        -- Additional outfits here...
    }
}
```

* **Description**: Allows customization of up to 3 clothing sets per gender.
* **Images**: Displays clothing preview in the menu.
* **Clothes Data**: Defines the clothing parts (e.g., torso, t-shirt, pants).

***

## <mark style="color:yellow;">**Vehicle Configuration**</mark>

```lua
Config.Vehicles = {
    --{ -- use it if you want to disable vehicle
    --    model = "none",
    --    price = 0,
    --    img = "https://cfx-nui-dh_poolCleaner/html/host/none.png",
    --},
    {
        model = "bodhi2",
        price = 100,
        img = "https://cfx-nui-dh_poolCleaner/html/host/bodhi2.png",
    },
    {
        model = "sandking",
        price = 300,
        img = "https://cfx-nui-dh_poolCleaner/html/host/sandking.png",
    },
}
```

<mark style="color:red;">If you want to turn off the work vehicle, use the example at the very top</mark>

* **Description**: Configures available rental vehicles.
* **Model**: Vehicle model name.
* **Price**: Cost of renting the vehicle.
* **Image**: URL for the vehicle image displayed in the menu.

***

## <mark style="color:yellow;">**Vehicle Spawn Points**</mark>

```lua
Config.VehiclesSpawnPoints = {
    vec4(853.3035, -895.6031, 25.3173, 282.2404),
}
```

* **Description**: Specifies the spawn locations for rented vehicles.
* **Coordinates**: Each `vec4` includes X, Y, Z position, and heading.

***

## <mark style="color:yellow;">**Notification Types**</mark>

```lua
Config.NotificationsTypes = {
    ["info"] = "https://cfx-nui-dh_poolCleaner/html/host/info.png",
    ["success"] = "https://cfx-nui-dh_poolCleaner/html/host/success.png",
    ["error"] = "https://cfx-nui-dh_poolCleaner/html/host/error.png",
}
```

* **Description**: Custom icons for different types of notifications.
* **Types**: `info`, `success`, `error`

***

## <mark style="color:yellow;">**HUD Placement**</mark>

```lua
Config.HudPlacement = "bottom-right"
Config.ControlButtonsPosition = "top-left"
```

* **Description**: Sets the position of the HUD on the screen.
* **Options**: `"bottom-left"`, `"bottom-right"`, `"top-right"`, `"top-left"`

***

## <mark style="color:yellow;">QuestsEnabled</mark>

```lua
Config.QuestsEnabled = true
```

***

## <mark style="color:yellow;">Disable Option</mark>

Disable task notification, task + 1 will not be shown on the screen if set to true

Disable help controls, help controls will not be shown on the screen if set to true

```lua
Config.DisableTaskNotification = false 
Config.DisableHelpControls = false 
```

***

## <mark style="color:yellow;">Blip</mark>

```lua
Config.PoolBlip = {
    sprite = 836,
    color = 5,
    scale = 0.8,
    name = "Tree",
}
```
