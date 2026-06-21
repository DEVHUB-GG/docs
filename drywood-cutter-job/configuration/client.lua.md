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
    name = "Miner Job",
}
```

* **Model**: Ped model used for the Miner NPC (`s_m_y_construct_02`).
* **Coordinates**: Position of the Miner NPC on the map.
* **Marker Details**:
  * **Sprite**: `527`
  * **Color**: `5`
  * **Scale**: `0.8`
* **Name**: Displayed as "Miner Job" when interacting.

***

## <mark style="color:yellow;">**Selling Configuration**</mark>

```lua
Config.Selling = {
    coords = vec3(-624.5422, -231.0968, 38.0570),
    sprite = 617,
    color = 5,
    scale = 0.8,
    name = "Selling",
}
```

* **Description**: Defines the location and appearance of the selling point.
* **Coordinates**: `(-624.5422, -231.0968, 38.0570)`
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
            img = "https://cfx-nui-dh_miner/html/host/miner_m1.png",
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
    --    img = "https://cfx-nui-dh_miner/html/host/none.png",
    --},
    {
        model = "bodhi2",
        price = 100,
        img = "https://cfx-nui-dh_miner/html/host/bodhi2.png",
    },
    {
        model = "sandking",
        price = 300,
        img = "https://cfx-nui-dh_miner/html/host/sandking.png",
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
    vec4(2827.2422, 2800.7427, 57.6350, 177.0261),
    vec4(2843.1853, 2790.4595, 59.6977, 212.7704),
    vec4(2839.3914, 2773.9546, 60.9099, 232.5432),
}
```

* **Description**: Specifies the spawn locations for rented vehicles.
* **Coordinates**: Each `vec4` includes X, Y, Z position, and heading.

***

## <mark style="color:yellow;">**Notification Types**</mark>

```lua
Config.NotificationsTypes = {
    ["info"] = "https://cfx-nui-dh_miner/html/host/info.png",
    ["success"] = "https://cfx-nui-dh_miner/html/host/success.png",
    ["error"] = "https://cfx-nui-dh_miner/html/host/error.png",
}
```

* **Description**: Custom icons for different types of notifications.
* **Types**: `info`, `success`, `error`

***

## <mark style="color:yellow;">**HUD Placement**</mark>

```lua
Config.HudPlacement = "bottom-right"
```

* **Description**: Sets the position of the HUD on the screen.
* **Options**: `"bottom-left"`, `"bottom-right"`, `"top-right"`, `"top-left"`

***

## <mark style="color:yellow;">QuestsEnabled</mark>

```lua
Config.QuestsEnabled = true
```

***

## <mark style="color:yellow;">TableSaw</mark>

```lua
Config.TableSaw = {
    blip = {
        coords = vec3(-578.3066, 5292.2622, 70.5579),
        sprite = 836,
        color = 5,
        scale = 0.8,
        name = "Table Saw",
    },
    position = {
        {
            coords = vec4(-578.8590, 5304.0098, 70.2606,  252.4474),
        },
        {
            coords = vec4(-575.9147, 5298.6309, 70.2606,  252.4474),
        },
        {
            coords = vec4(-584.9834, 5294.3555, 70.2437,  252.4474),
        },
        {
            coords = vec4(-568.5790, 5272.6123, 70.2480,  252.4474),
        },
        
    }
}
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
Config.TreeBlip = {
    sprite = 836,
    color = 5,
    scale = 0.8,
    name = "Tree",
}
```
