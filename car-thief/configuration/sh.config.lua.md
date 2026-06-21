---
description: Documentation for sh.config.lua Configuration
---

# sh.config.lua

The main gameplay configuration. Long lists (vehicle models, colors, names, coordinates) are trimmed below to a few representative entries — open the file to see the full lists. Never rename the `Config.*` keys; only change their values.

## <mark style="color:yellow;">**Debug**</mark>

```lua
Config.Debug = true
```

* **Description**: Enables debug prints to help you track down issues. Set to `false` on a live server.

***

## <mark style="color:yellow;">**SfxVolume**</mark>

```lua
Config.SfxVolume = {
    close = 0.1,
    error = 0.5,
    open = 0.1,
    success = 0.5,
    terminal = 0.5,
}
```

* **Description**: Volume (`0.0` – `1.0`) of the UI sound effects played by the device interfaces.
* **Fields**: `close`, `error`, `open`, `success`, `terminal` — one per UI sound.

***

## <mark style="color:yellow;">**BlipSettings**</mark>

```lua
Config.BlipSettings = {
    sprite = 225,
    color = 5,
    scale = 0.8,
}
```

* **Description**: Default blip style used for the car meet location.
* **Fields**: `sprite`, `color`, `scale` — see the [FiveM blip references](https://docs.fivem.net/docs/game-references/blips/).

***

## <mark style="color:yellow;">**Peds**</mark>

```lua
Config.Peds = {
    { model = GetHashKey("a_m_m_socenlat_01"), gender = "male" },
    { model = GetHashKey("a_f_y_business_02"), gender = "female" },
    -- ... more guest peds
}
```

* **Description**: Pool of NPC "guests" that spawn at the car meet to make it feel busy.
* **Fields**:
  * **model**: the ped model hash.
  * **gender**: `"male"` or `"female"` — used to pick a matching name from `Config.Names`.

***

## <mark style="color:yellow;">**PedAnimations**</mark>

```lua
Config.PedAnimations = {
    { dict = "amb@world_human_bum_wash@male@low@idle_a", anim = "idle_a" },
    -- ... more animations
}
```

* **Description**: Idle animations the car meet guests play, picked at random.
* **Fields**: `dict` (animation dictionary) and `anim` (animation name).

***

## <mark style="color:yellow;">**VehiclesSettings**</mark>

```lua
Config.VehiclesSettings = {
    models = {
        {
            model = GetHashKey("adder"),
            label = "Adder",
            class = "Sports",
            yearOfProduction = { 2010, 2016 }, -- min, max
            image = "https://docs.fivem.net/vehicles/adder.webp",
        },
        -- ... more vehicles
    },
    registries = { "PL", "UK", "DE", "ES", "IT", "FR" },
    colors = {
        { id = 0, label = "Metallic Black" },
        { id = 1, label = "Metallic Graphite Black" },
        -- ... full GTA color palette
    },
    insuranceChance = 0.5,
    registrationChance = 0.5,
}
```

* **Description**: Everything about the cars that spawn at the meet and the fake registry data shown in the Car Checker.
* **Fields**:
  * **models**: the pool of cars that can be parked at the meet.
    * **model**: vehicle model hash.
    * **label**: name shown in the registry terminal.
    * **class**: vehicle class label.
    * **yearOfProduction**: `{ min, max }` — a random year in this range is generated.
    * **image**: preview image shown in the terminal.
  * **registries**: country codes used to build fake number plates / registry entries in the UI.
  * **colors**: list of `{ id, label }` GTA colour entries used to label the vehicle's colour in the terminal.
  * **insuranceChance**: `0.0` – `1.0` chance the scanned vehicle shows as insured (`0.5` = 50%).
  * **registrationChance**: `0.0` – `1.0` chance the scanned vehicle shows a valid registration (`0.5` = 50%).

***

## <mark style="color:yellow;">**Nodes**</mark>

```lua
Config.Nodes = { "WARSAW", "LONDON", "BERLIN", "MADRID", "ROME", "PARIS" }
```

* **Description**: Pool of "node" names shown in the registry terminal UI for flavour.

***

## <mark style="color:yellow;">**Names**</mark>

```lua
Config.Names = {
    male = { "Liam Anderson", "Noah Thompson", -- ... },
    female = { "Emma Johnson", "Olivia Davis", -- ... },
}
```

* **Description**: Names assigned to the car meet guests (and the target owner). Split by gender so a ped gets a fitting name.

{% hint style="info" %}
There must be **at least as many names as peds** at a location. Each location's `amountOfPeds` must be lower than or equal to the number of names available.
{% endhint %}

***

## <mark style="color:yellow;">**Music**</mark>

```lua
Config.Music = {
    volume = 0.1,
    distance = 35.0,
    musics = {
        "https://youtu.be/kS87s-Gsgrs?si=RUAE8Uy6uwwxFRCs",
        -- ... more tracks
    },
}
```

* **Description**: Background music played at the car meet.
* **Fields**:
  * **volume**: `0.0` – `1.0` playback volume.
  * **distance**: range in meters at which the music can be heard (a wider range also smooths the fade as the player approaches).
  * **musics**: list of track URLs — one is picked at random and played at the meet.

***

## <mark style="color:yellow;">**CarThiefLocations**</mark>

```lua
Config.CarThiefLocations = {
    {
        blipCoords = vec3(-1666.841, -925.729, 8.042),
        amountOfPeds = 7,
        vehicleLocations = {
            vec4(-1672.297, -930.277, 7.401, 319.127),
            -- ... more vehicle spots
        },
        pedLocations = {
            vec4(-1666.777, -926.222, 8.032, 68.326),
            -- ... more ped spots
        }
    },
    -- ... more locations
}
```

* **Description**: The car meet locations. The script picks one at random when a task starts.
* **Fields**:
  * **blipCoords**: `vec3` location of the blip shown to the player.
  * **amountOfPeds**: how many guest peds spawn here (must be ≤ the number of `pedLocations` and ≤ the number of names in `Config.Names`).
  * **vehicleLocations**: `vec4` spots where cars are parked.
  * **pedLocations**: `vec4` spots where the guests stand.

***

## <mark style="color:yellow;">**CarChecker**</mark>

```lua
Config.CarChecker = {
    itemName = "devhub_car_checker",

    duiConfig = { -- Prefer to keep the default values
        url    = "nui://devhub_carThief/html/index.html",
        resX   = 1280,
        resY   = 720,
        model  = GetHashKey("prop_laptop_lester"),
        -- screen mapping / texture swap values
    },

    cameraConfig = {
        offset = vec3(0.0, -0.28, 0.165),
        fadeTime = 500,
    },

    plateFocus = {
        key          = "Tab",
        searchRadius = 60.0,
        camDistance  = 1.6,
        camHeight    = 0.2,
    },

    foundSequence = {
        closeDelay = 4000,
    },

    animation = { dict = "anim@heists@box_carry@", anim = "idle" },
    attach = {
        bone = 28422,
        offset = vec3(0.0, -0.1, -0.18),
        rot = vec3(-2.0, 0.0, 180.0),
    },
}
```

* **Description**: The Car Checker item that opens the Vehicle Registry Terminal used to identify the target car.
* **Fields**:
  * **itemName**: inventory item that triggers the car checker when used.
  * **duiConfig**: advanced device-screen settings (resolution, prop model and screen calibration). Leave at default unless you know what you are doing — these are tuned to the bundled prop.
  * **cameraConfig**: `offset` and `fadeTime` (ms) for the camera while the device is open.
  * **plateFocus**: the "look at the nearest plate" helper.
    * **key**: keyboard key that toggles plate focus (a JS `KeyboardEvent.key` value, e.g. `"Tab"`, `"f"`).
    * **searchRadius**: how far (m) to search for the closest vehicle.
    * **camDistance** / **camHeight**: how far behind and above the plate the camera sits.
  * **foundSequence.closeDelay**: ms the result stays on screen before the device auto-closes.
  * **animation** / **attach**: how the device is held in the player's hand (tune the offset/rotation in-game if it clips).

***

## <mark style="color:yellow;">**KeyCloner**</mark>

```lua
Config.KeyCloner = {
    itemName = "devhub_key_cloner",

    duiConfig = { -- Prefer to keep the default values
        url   = "nui://devhub_carThief/html/index.html",
        resX  = 720,
        resY  = 1280,
        model = GetHashKey("prop_phone_cs_frank"),
    },

    initialization = {
        updateInterval = 50,
        distanceThreshold = 9.0,
        maxRatePerSecond = 20.0,
        minDistance = 2.0,
        detectionBarIncreaseRate = 2.5,
        detectionBarDecreaseRate = 2.5,
        pedMarker = { --[[ marker styling ]] },
    },

    animation = { dict = "cellphone@", anim = "cellphone_text_read_base" },
    attach = { bone = 28422, offset = vec3(0.0, 0.0, 0.0), rot = vec3(0.0, 0.0, 0.0) },
    cameraConfig = { offset = vec3(0.0, -0.2, 0.0), fadeTime = 500 },

    distractingPed = {
        target = { icon = 'fa-solid fa-comment' },
        npcDialogData = { --[[ dialog styling ]] },
        distractedPedCoords = vec4(0.245, 0.275, 0.110, 31.899),
        distractTime = 60000,
        staticMessagePosition = "bottom-right",
    },

    unlockingCar = {
        vehicleMarker = { --[[ marker styling ]] },
        maxDistance = 5.0,
        duration = 7500,
    },
}
```

* **Description**: The Key Cloner item used to read the key, distract the owner and unlock the car.
* **Fields**:
  * **itemName**: inventory item that triggers the key cloner when used.
  * **duiConfig**: advanced device-screen settings for the phone prop. Leave at default.
  * **initialization**: the key-reading mini-game.
    * **updateInterval**: ms between progress updates.
    * **distanceThreshold**: distance (m) beyond which reading progress starts to decrease if the player walks away from the car.
    * **maxRatePerSecond**: how much progress is added per second when within range.
    * **minDistance**: if the player is closer to the owner than this, the **detection bar** starts to rise.
    * **detectionBarIncreaseRate** / **detectionBarDecreaseRate**: how fast detection rises (too close) or falls (far enough), per `updateInterval`.
    * **pedMarker**: marker drawn over the owner.
  * **animation** / **attach** / **cameraConfig**: how the phone is held and framed.
  * **distractingPed**: the "lure the owner away" interaction.
    * **distractTime**: how long (ms) the owner stays distracted (default `60000` = 60s).
    * **staticMessagePosition**: where the on-screen distraction timer is shown — `"top-left"`, `"top-right"`, `"bottom-left"`, `"bottom-right"`.
    * The dialog presents 3 random lines each time, **at least one of which always fails** (cancels the meet and calls the police).
  * **unlockingCar**:
    * **maxDistance**: max distance (m) from the car at which it can be unlocked.
    * **duration**: time (ms) to unlock the car (default `7500` = 7.5s).

***

## <mark style="color:yellow;">**ReturningVehicle**</mark>

```lua
Config.ReturningVehicle = {
    blip = { sprite = 225, color = 5, scale = 0.8 },

    locations = {
        {
            ped = {
                model = GetHashKey("a_m_m_soucent_01"),
                anim = { dict = "missmic_3_ext@leadin@mic_3_ext", anim = "_leadin_trevor" },
                coords = vec4(-693.550, -1396.158, 4.150, 230.969),
                name = "Bob Grass",
            },
            targetVehicleCoords = vec3(-680.220, -1403.692, 4.576),
        },
        -- ... more drop-off locations
    },

    target = { icon = 'fa-solid fa-comment', radius = 2.0 },
    parkRadius = 10.0,
    npcDialogData = { --[[ dialog styling ]] },
}
```

* **Description**: Where and how the player hands the stolen car over to the buyer to finish the job.
* **Fields**:
  * **blip**: blip style for the drop-off.
  * **locations**: drop-off points, one chosen at random.
    * **ped**: the buyer NPC — `model`, idle `anim`, spawn `coords` and the `name` shown in the UI.
    * **targetVehicleCoords**: where the player must bring the vehicle.
  * **target**: interaction icon and radius.
  * **parkRadius**: how close (m) the vehicle must be parked to the drop-off for it to count.

***

## <mark style="color:yellow;">**Informant**</mark>

```lua
Config.Informant = {
    despawnDelay = 60000,
    ped = {
        model = "a_m_y_business_03",
        anim = { dict = "amb@world_human_smoking@male@male_a@idle_a", anim = "idle_a" },
    },
    locations = {
        vec4(269.1508, 3.2266, 78.2459, 246.6737),
        vec4(-1564.5082, -300.3329, 47.2321, 320.0967),
    },
    blip = { sprite = 480, color = 5, scale = 0.8 },
    target = { icon = "fas fa-user-secret" },
    giveItems = {
        { name = "devhub_car_checker", amount = 1 },
        { name = "devhub_key_cloner", amount = 1 },
    },
    npcDialogData = { --[[ dialog styling ]] },
}
```

* **Description**: The contact the player must meet before the car meet location is revealed.
* **Fields**:
  * **despawnDelay**: ms the contact lingers after the conversation before despawning.
  * **ped**: informant model and idle animation (set `anim` to `false` to disable).
  * **locations**: spawn points, one picked at random per task.
  * **blip** / **target**: blip and interaction styling.
  * **giveItems**: tools handed to the player if they don't already carry them (each `{ name, amount }`).

***

## <mark style="color:yellow;">**LocationResetTime**</mark>

```lua
Config.LocationResetTime = 600000
```

* **Description**: Time (ms) after which a used location resets and becomes available again.
* **Example**: `600000` = 10 minutes.

***

## <mark style="color:yellow;">**MinPolice**</mark>

```lua
Config.MinPolice = 0
```

* **Description**: Minimum number of police online required to join the queue. `0` = no requirement.

***

## <mark style="color:yellow;">**MaxTaskTime &amp; PersonalCooldownTime**</mark>

```lua
Config.MaxTaskTime = 20
Config.PersonalCooldownTime = 10
```

* **MaxTaskTime**: maximum time, in **minutes**, before the next queued player is processed.
* **PersonalCooldownTime**: cooldown, in **minutes**, before a player can join the queue again after finishing.

***

## <mark style="color:yellow;">**StartRequirements**</mark>

```lua
Config.StartRequirements = {
    -- Item example (player must have the item, set consume = true to remove it on start):
    -- { name = "devhub_car_checker", amount = 1, consume = false },

    -- Custom handler example:
    -- {
    --     label = "Crew Membership",
    --     icon = "fas fa-users",
    --     check = function(source) return true end,
    --     action = function(source) end,
    -- },
}
```

* **Description**: Optional conditions a player must meet to start a job. Empty by default (no requirements).
* **Entry types**:
  * **Item requirement**: `{ name, amount, consume }` — require an item, optionally removing it on start.
  * **Custom handler**: a `label` + `icon` shown to the player, a `check(source)` returning whether the player qualifies, and an `action(source)` that runs once the task actually starts.

***

## <mark style="color:yellow;">**PedSettings**</mark>

```lua
Config.PedSettings = {
    enabled = true,
    peds = {
        {
            model = "ig_lestercrest",
            coords = vec4(-452.5981, 171.7005, 75.2038, 350.6036),
            blip = {
                enabled = true,
                sprite = 225,
                color = 5,
                scale = 0.8,
                label = "Car Meet Contact",
            },
        },
    },
}
```

* **Description**: The in-world NPC players can talk to in order to join the queue or check their position (an alternative to the laptop).
* **Fields**:
  * **enabled**: spawn the queue NPC at all.
  * **peds**: each NPC's `model`, `coords`, and a `blip` (`enabled`, `sprite`, `color`, `scale`, `label`).

***

## <mark style="color:yellow;">**SkillTree**</mark>

```lua
Config.SkillTree = {
    enabled = false,
    category = "laptop",
    xpPerJob = 30,
    skills = {
        steadyHands  = "carthief_steady_hands",
        quickHands   = "carthief_quick_hands",
        silverTongue = "carthief_silver_tongue",
        masterThief  = "carthief_master_thief",
    },
}
```

* **Description**: [devhub\_skillTree](https://store.devhub.gg/product/6438994) integration — XP rewards and skill perks.
* **Fields**:
  * **enabled**: turn the skill tree XP rewards and perks on.
  * **category**: the skill tree category UID the skills and XP belong to.
  * **xpPerJob**: XP awarded when a job is completed.
  * **skills**: maps each perk to the `uid` of a skill in your skill tree. When a player has the skill unlocked, its effect is applied:
    * **steadyHands**: key cloner detection meter fills slower.
    * **quickHands**: key cloner reads the key faster.
    * **silverTongue**: distracted owners stay away longer.
    * **masterThief**: the stolen vehicle unlocks faster.

***

## <mark style="color:yellow;">**Rewards**</mark>

```lua
Config.Rewards = {
    enabled = false,
    money = {
        account = "bank",
        min = 1500,
        max = 3000,
    },
    items = {
        -- { item = "goldbar", min = 1, max = 2, chance = 40 },
        -- { item = "rolex",   min = 1, max = 1, chance = 25 },
    },
}
```

* **Description**: Optional reward paid out directly when the stolen car is returned. **Does not require devhub\_laptop.**
* **Fields**:
  * **enabled**: hand out these rewards on completion.
  * **money**: cash payout — `account` (`"cash"` or `"bank"`), `min` and `max` (set `max = 0` to skip money).
  * **items**: item rewards — each entry independently rolls its `chance` (1–100); on success the player receives `min`–`max` of that `item`.

***

## <mark style="color:yellow;">**LaptopConfig**</mark>

```lua
Config.LaptopConfig = {
    enabled = true,
    traderId = 'trader_1',
    name = 'Car Thief',
    smallDescription = 'Steal a high-end car from an underground car meet.',
    description = 'Join the queue and wait for your turn. Travel to the car meet, ...',
    bannerImage = 'https://upload.devhub.gg/.../carThief/taskImage.webp',
    images = { '.../img1.webp', '.../img2.webp', '.../img3.webp' },
    link = "https://store.devhub.gg",
    rewards = {
        items = {},
        money = 2000,
        crimePoints = 600,
        xp = 300,
    },
    tasks = {
        ['uid_1'] = { name = 'Join the queue', description = '...', icon = 'fas fa-sign-in-alt', order = 1 },
        -- ... uid_8, uid_2 ... uid_7 (one entry per step, ordered)
    },
}
```

* **Description**: Integration with the [devhub\_laptop](https://devhub.gg/product/7054272) **Crime Traders** app — how the job is listed and what it pays out via the laptop.
* **Fields**:
  * **enabled**: register the job with the laptop's Crime Traders app.
  * **traderId**: the trader category the job appears under.
  * **name** / **smallDescription** / **description**: text shown in the app.
  * **bannerImage** / **images**: artwork shown in the app gallery.
  * **link**: store link shown in the app.
  * **rewards**: paid out via the laptop on completion — `money`, `crimePoints`, `xp` and optional `items`.
  * **tasks**: the checklist steps shown to the player, each with a `name`, `description`, `icon` and `order` (the `uid_*` keys are the task identifiers; keep them unique).

***
