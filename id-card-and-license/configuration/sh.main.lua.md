---
description: Documentation for sh.main.lua Configuration
---

# sh.main.lua

## <mark style="color:yellow;">**Debug**</mark>

```lua
Config.Debug = false -- Enable debug prints
```

* **Description**: Enables verbose `debugPrint` output in the server console and NUI. Leave off in production.

***

## <mark style="color:yellow;">**Dev**</mark>

```lua
Config.Dev = false
```

* **Description**: Enables development-only features such as the terminal demo target and the `/testscreenshot` command. <mark style="color:red;">Not meant for production — may cause issues on a live server.</mark>

***

## <mark style="color:yellow;">**NotifyOnExportFailure**</mark>

```lua
Config.NotifyOnExportFailure = true
```

* **Description**: When `true` (default), any [export](../exports-and-events.md) that fails also shows the affected player a `Core.Notify` explaining why (e.g. _"Your license data is still loading."_). The exports still return their `success, error` values regardless — this only controls the automatic notification.
* **Who is notified**: On the **client**, the local player who called the export. On the **server**, the `source` player passed to the export; system calls (`source` `0`/`nil`) and the template/metadata exports (which have no player) never notify. The side-effect-free check `isPlayerLoaded` never notifies either.
* **When to turn it off**: If your own integration already messages the player on failure, or if you call these exports from loops/polling where repeated notifications would spam the player.
* **Messages**: Pulled from the `export_fail_*` keys in [`sh.lang.lua`](sh.lang.lua.md) — edit those to reword or translate them.

***

## <mark style="color:yellow;">**RetakeableLicenseStatus**</mark>

```lua
-- License statuses: active, expired, revoked, lost, suspended
Config.RetakeableLicenseStatus = {
    ["expired"] = true,
    ["revoked"] = true,
    ["lost"] = true,
}
```

* **Description**: Which license statuses allow a player to pick the license up again at the city hall NPC. Only the listed statuses are retakeable; any status set to `true` is allowed.

***

## <mark style="color:yellow;">**LicensePickUp**</mark>

```lua
Config.LicensePickUp = {
    coords = vec4(-545.2302, -203.6606, 38.2152, 208.0829),
    model = "a_m_y_business_03",
    blip = {
        enabled = true,
        sprite = 408, -- License/ID card icon
        color = 2, -- Green
        scale = 0.8,
        label = "License Pickup",
    },
}
```

* **coords**: Location and heading (`vec4` = X, Y, Z, heading) of the license pickup NPC (the city clerk).
* **model**: Ped model used for the NPC.
* **blip**:
  * **enabled**: Whether to show a map blip.
  * **sprite**: Blip sprite ID (`408` = license/ID card).
  * **color**: Blip color (`2` = green).
  * **scale**: Blip size.
  * **label**: Map label text.

***

## <mark style="color:yellow;">**LicensePositionEditor**</mark>

```lua
Config.LicensePositionEditor = true
```

* **Description**: When enabled, adds a third option to open the position editor for the currently shown license, letting players adjust where the license displays on screen.

***

## <mark style="color:yellow;">**LicenseShowDistance**</mark>

```lua
Config.LicenseShowDistance = 10.0
```

* **Description**: Maximum distance (in game units) at which nearby players can see a license being shown.

***

## <mark style="color:yellow;">**LicenseShowCooldown**</mark>

```lua
Config.LicenseShowCooldown = 5
```

* **Description**: Cooldown in seconds between showing licenses, to prevent spam.
* **Example**: `5` means a player must wait 5 seconds between shows. Set to `0` to disable the cooldown.

***

## <mark style="color:yellow;">**UsePhysicalLicenses**</mark>

```lua
Config.UsePhysicalLicenses = false
```

* **Description**: When `true`, licenses behave as physical inventory items (requires item metadata support in `devhub_lib`). <mark style="color:red;">Enabling this disables virtual licenses — the `/licenses` and `/favorites` commands will not work.</mark> The item used per license is configured in the license creator UI under general settings.

***

## <mark style="color:yellow;">**MaxFavoriteLicenses**</mark>

```lua
Config.MaxFavoriteLicenses = 5
```

* **Description**: Maximum number of favorite licenses a player can mark.
* **Example**: Maximum allowed value is `5`.

***

## <mark style="color:yellow;">**Usage**</mark>

```lua
Config.Usage = {
    favorites = { -- Open favorite licenses hand
        command = "favorites",
        item    = "devhub_favlicenses",
        event   = "devhub_licenses:client:openFavorites",
        keybind = false,
    },
    licenses = { -- Open license holder / card holder
        command = "licenses",
        item    = "devhub_licenseholder",
        event   = "devhub_licenses:client:openLicenses",
        keybind = false,
    },
    licenseMdt = { -- Show license mdt
        command = "licenseMdt",
        item    = "devhub_licensesmdt",
        event   = "devhub_licenses:client:openMainMenu",
        keybind = false,
    },
}
```

* **Description**: How each interface can be opened. Every entry supports four trigger methods — set any to `false` to disable it.
  * **command**: Chat command name.
  * **item**: Usable inventory item name.
  * **event**: Client event name that opens the interface.
  * **keybind**: Key to bind (e.g. `"F6"`, `"G"`, `"COMMA"`) or `false`. Uses `RegisterKeyMapping`, so players can rebind it under FiveM Settings → Key Bindings.
* **favorites**: Opens the favorite licenses hand display.
* **licenses**: Opens the player's card holder.
* **licenseMdt**: Opens the license MDT tablet.

***

## <mark style="color:yellow;">**PlayerTarget**</mark>

```lua
Config.PlayerTarget = {
    enabled = true,
    icon = "fas fa-id-card",
    label = "Check Licenses",
    allowTakeLicense = true,
    animation = {
        dict = "mini@repair",
        anim = "fixing_a_ped",
        blendIn = 8.0,
        blendOut = 8.0,
        duration = -1,
        flag = 1,
    },
}
```

* **enabled**: Adds a target option on other players to view their licenses.
* **icon**: Target icon (Font Awesome class).
* **label**: Target label text.
* **allowTakeLicense**: Whether players may steal/take licenses from another player's card holder.
* **animation**: Animation played while searching a player's licenses.
  * **dict**: Animation dictionary.
  * **anim**: Animation name.
  * **blendIn** / **blendOut**: Blend speeds.
  * **duration**: Duration in ms (`-1` = loop until stopped).
  * **flag**: Animation flag (`1` = loop).

***

## <mark style="color:yellow;">**SuspensionCheckIntervalHours**</mark>

```lua
Config.SuspensionCheckIntervalHours = 1
```

* **Description**: How often (in hours) the server checks for suspended licenses whose suspension period has ended.

***

## <mark style="color:yellow;">**ExpirationCheckIntervalHours**</mark>

```lua
Config.ExpirationCheckIntervalHours = 1
```

* **Description**: How often (in hours) the server checks for and processes expired licenses.

***

## <mark style="color:yellow;">**UseLaptopApp**</mark>

```lua
Config.UseLaptopApp = false
```

* **Description**: When `true`, the license system runs as an app inside the free `devhub_laptop` resource ([store.devhub.gg/product/7054272](https://store.devhub.gg/product/7054272)) instead of a standalone UI.

***

## <mark style="color:yellow;">**LaptopApp**</mark>

```lua
Config.LaptopApp = {
    label = "Licenses",
    img = "https://cfx-nui-devhub_laptop/html/images/apps/licenses.png",
    path = "https://cfx-nui-devhub_licenses/html/index.html",
    category = "premium",
    rating = 5,
    description = "Licenses application for managing your licenses",
    longDescription = "A comprehensive platform where you can view, manage, and track your licenses with detailed information and status updates.",
    size = 52,
    downloads = 1000,
    fullScreen = true,
    reviews = {
        {
            user = "Tom",
            rating = 5,
            comment = "The license system is fantastic! It keeps everything organized and easy to manage.",
            date = "2025-08-14"
        },
        {
            user = "Sarah",
            rating = 4,
            comment = "I love the interface and how easy it is to check my licenses.",
            date = "2025-08-15"
        },
    }
}
```

* **Description**: Metadata shown for the app inside the `devhub_laptop` store/launcher. Only used when `Config.UseLaptopApp` is `true`.
  * **label**: App name shown on the laptop.
  * **img**: App icon URL.
  * **path**: URL the app loads.
  * **category** / **rating** / **size** / **downloads**: Cosmetic store metadata.
  * **fullScreen**: Whether the app opens full screen.
  * **reviews**: Sample reviews shown on the app's store page.

***

## <mark style="color:yellow;">**ItemShop**</mark>

```lua
Config.ItemShop = {
    paymentMethod = "both", -- "cash", "card" or "both"
    items = {
        { name = 'devhub_licenseholder', price = 500, max = 1 },
        { name = 'devhub_licensesmdt',    price = 750, max = 1 },
        { name = 'devhub_favlicenses',    price = 300, max = 1 },
    },
}
```

* **Description**: Items the License Pickup NPC sells.
* **paymentMethod**: Accepted payment — `"cash"`, `"card"`, or `"both"`.
* **items**: Each entry defines an item:
  * **name**: Inventory item name.
  * **price**: Cost.
  * **max**: Maximum quantity a player can buy.

***

## <mark style="color:yellow;">**UndergroundItemShop**</mark>

```lua
Config.UndergroundItemShop = {
    paymentMethod = "cash", -- "cash", "card" or "both"
    items = {
        { name = 'devhub_fakelicenses', price = 1500, max = 1 },
    },
}
```

* **Description**: Items sold by the underground NPC (John). Same field structure as `Config.ItemShop`.

***

## <mark style="color:yellow;">**CreatorOnlyForAdmins**</mark>

```lua
Config.CreatorOnlyForAdmins = true
```

* **Description**: When `true`, only admins can open the license creator. Uses `Core.IsPlayerAdmin` from `devhub_lib`.

***

## <mark style="color:yellow;">**HideoutLocations**</mark>

```lua
Config.HideoutLocations = {
    {
        coords = vec4(393.3414, 3000.3530, 39.5347, 150.4899),
        spawnDistance = 200.0,
    },
}
```

* **Description**: Entrances to the fake ID underground hideout. Each entry:
  * **coords**: Surface location and heading of the hideout entrance.
  * **spawnDistance**: Distance at which the hideout interior props spawn.

***

## <mark style="color:yellow;">**UndergroundNpc**</mark>

```lua
Config.UndergroundNpc = {
    model = "s_m_y_ammucity_01",
    offset = vec3(11.2125, -3.5820, -8.1847), -- Offset from underground parent entity
    heading = 46.3,
}
```

* **Description**: The underground contact NPC (John).
  * **model**: Ped model.
  * **offset**: Position relative to the underground parent entity.
  * **heading**: Heading relative to the underground parent heading.

***

## <mark style="color:yellow;">**Papercutter / Foiltipper / Embosser**</mark>

```lua
Config.Papercutter = {
    offset = vec3(19.44, -1.4, -8.19), -- between the two desks
    heading = 235.0,
}

Config.Foiltipper = {
    offset = vec3(17.04, -5.10, -7.39), -- left desk
    heading = 235.0,
}

Config.Embosser = {
    offset = vec3(22.04, 2.00, -7.40), -- right desk
    heading = 235.0,
}
```

* **Description**: Placement of the three fake-license crafting stations inside the hideout.
  * **offset**: Position relative to the underground parent entity.
  * **heading**: Heading relative to the underground parent heading.

***

## <mark style="color:yellow;">**FakeIdPhotos**</mark>

```lua
Config.FakeIdPhotos = {
    'https://upload.devhub.gg/dh_upload/licenses/fakeIdPhoto1.jpg',
    'https://upload.devhub.gg/dh_upload/licenses/fakeIdPhoto2.jpg',
    -- ... up to fakeIdPhoto8.jpg
}
```

* **Description**: List of avatar photo URLs players can choose from when forging a fake ID.

***

## <mark style="color:yellow;">**FakeIdMissionConfig**</mark>

```lua
Config.FakeIdMissionConfig = {
    deliveryLocations = {
        { coords = vec4(-683.3203, -172.8938, 36.8213, 295.9203) },
        { coords = vec4(-276.5885, 303.0736, 89.7183, 344.4300) },
        { coords = vec4(845.5369, -962.2592, 25.5211, 279.7717) },
        { coords = vec4(967.6248, -1828.8547, 30.2365, 352.4440) },
    },
    npcModel = {
        "a_m_m_hillbilly_02",
        "a_m_o_soucent_02",
        "a_m_o_soucent_03",
        "a_m_o_tramp_01",
    },
    payMin = 500,
    payMax = 2000,
    blipSprite = 280,
    blipColor = 5,
    blipScale = 0.85,
}
```

* **Description**: Settings for the fake-license delivery mission.
  * **deliveryLocations**: Possible drop-off points; one is chosen at random per mission.
  * **npcModel**: Pool of buyer ped models; one is chosen at random.
  * **payMin** / **payMax**: Reward range paid on completion.
  * **blipSprite** / **blipColor** / **blipScale**: Map blip styling for the delivery target.
