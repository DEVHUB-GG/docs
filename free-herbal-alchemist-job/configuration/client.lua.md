---
description: Documentation for client.lua Configuration
---

# client.lua

## <mark style="color:yellow;">**Herbs Spawn Distance**</mark>

```lua
Config.HerbsSpawnDistance = 45.0
```

**Description:**\
Defines the maximum distance from the player within which herbs will spawn.

**Example:**\
A value of `45.0` means herbs can spawn within a 45-meter radius of the player.

***

## <mark style="color:yellow;">**Clothing Configuration**</mark>

```lua
Config.Clothing = {
    ["mp_m_freemode_01"] = {
        {
            img = "https://cfx-nui-devhub_herbalAlchemist/html/host/alchemist_m1.png",
            clothes = {
                torso = {95, 1},
                tshirt = {15, 0},
                pants = {90, 0},
                boots = {63, 4},
                gloves = {26, 0},
            }
        },
        -- More male clothing sets here...
    },
    ["mp_f_freemode_01"] = {
        {
            img = "https://cfx-nui-devhub_herbalAlchemist/html/host/alchemist_f1.png",
            clothes = {
                torso = {49, 1},
                tshirt = {15, 0},
                pants = {93, 2},
                boots = {66, 4},
                gloves = {20, 0},
            }
        },
        -- More female clothing sets here...
    }
}
```

**Description:**\
Allows up to 3 clothing sets per gender. Each set includes a preview image and detailed outfit settings.

**Parameters:**

* **`img`:** URL of the clothing preview image.
* **`clothes`:** Defines the outfit parts (e.g., torso, t-shirt, pants, etc.).

***

## <mark style="color:yellow;">**Vehicle Configuration**</mark>

```lua
Config.Vehicles = {
    -- { -- Uncomment this block to completely disable vehicles
    --     model = "none",
    --     price = 0,
    --     img = "https://cfx-nui-devhub_herbalAlchemist/html/host/none.png",
    -- },
    {
        model = "mesa",
        price = 0,
        img = "https://cfx-nui-devhub_herbalAlchemist/html/host/mesa.png",
    },
    {
        model = "freecrawler",
        price = 300,
        img = "https://cfx-nui-devhub_herbalAlchemist/html/host/freecrawler.png",
    }
}

```

**Description:**\
Configures up to two rental vehicles. If you prefer not to use vehicles at all, you can completely disable them by uncommenting the `model = "none"` block.

**Details:**

* **Model:** The name of the vehicle model.
* **Price:** Cost of renting the vehicle.
* **Image:** URL for the vehicle's preview image displayed in the menu.

### **Disable Vehicles**

Uncomment the following block:

```lua
{
    model = "none",
    price = 0,
    img = "https://cfx-nui-devhub_herbalAlchemist/html/host/none.png",
}
```

This will remove vehicles from the system entirely.

***

## <mark style="color:yellow;">**Vehicle Spawn Points**</mark>

```lua
Config.VehiclesSpawnPoints = {
    vec4(1537.0837, 2177.8159, 78.4643, 175.1745),
    vec4(1552.5822, 2193.8521, 78.4341, 357.4615),
    vec4(1558.9625, 2203.4861, 78.4950, 90.5871),
}
```

**Description:**\
Defines the spawn points for rented vehicles. Each point includes coordinates (X, Y, Z) and a heading.

**Example Point:**\
`vec4(1537.0837, 2177.8159, 78.4643, 175.1745)`

* X: `1537.0837`
* Y: `2177.8159`
* Z: `78.4643`
* Heading: `175.1745`

***

## <mark style="color:yellow;">**Main Ped Configuration**</mark>

```lua
Config.MainPed = {
    model = "s_m_m_cntrybar_01",
    coords = vec4(1546.0927, 2166.5562, 78.7278, 89.7710),
    sprite = 140,
    color = 5,
    scale = 0.8,
    name = "Herbal Alchemist Job",
}
```

**Description:**\
Defines the main NPC for the Herbal Alchemist job.

**Details:**

* **Model:** `"s_m_m_cntrybar_01"`
* **Coordinates:** NPC spawn location in `vec4` format.
* **Marker:**
  * Sprite: `140`
  * Color: `5`
  * Scale: `0.8`
* **Name:** Displays as `"Herbal Alchemist Job"` when interacting.

***

## <mark style="color:yellow;">**Alchemy Tables**</mark>

```lua
Config.Tables = {
    coords = {
        vec4(252.2556, 3096.8608, 42.4300, 7.1345),
        vec4(-313.0403, 6310.4414, 32.4401, 220.8948),
        -- More table locations...
    },
    sprite = 499,
    color = 5,
    scale = 0.8,
    name = "# Alchemist Table",
}
```

**Description:**\
Defines the locations of alchemy tables for processing herbs.

**Marker Details:**

* Sprite: `499`
* Color: `5`
* Scale: `0.8`
* Name: Displays as `"# Alchemist Table"` when interacting.

***

## <mark style="color:yellow;">**Delivery Points**</mark>

```lua
Config.Delivery = {
    vec3(2704.192383, 4330.427734, 45.852066),
    vec3(2728.882080, 4280.732910, 48.961315),
    -- More delivery locations...
}
```

**Description:**\
Lists delivery locations for transporting finished products.

**Example Location:**\
`vec3(2704.192383, 4330.427734, 45.852066)`

* X: `2704.192383`
* Y: `4330.427734`
* Z: `45.852066`

***

## <mark style="color:yellow;">**Notification Types**</mark>

```lua
Config.NotificationsTypes = {
    ["info"] = "https://cfx-nui-devhub_herbalAlchemist/html/host/info.png",
    ["success"] = "https://cfx-nui-devhub_herbalAlchemist/html/host/success.png",
    ["error"] = "https://cfx-nui-devhub_herbalAlchemist/html/host/error.png",
}
```

**Description:**\
Specifies icons for different notification types.

**Types:**

* **Info:** Displays general information.
* **Success:** Shown after completing tasks.
* **Error:** Alerts for failed actions.

***

## <mark style="color:yellow;">**HUD Placement**</mark>

```lua
Config.HudPlacement = "bottom-right"
```

**Description:**\
Sets the position of the HUD on the screen.

**Options:**

* `"bottom-left"`
* `"bottom-right"`
* `"top-right"`
* `"top-left"`
