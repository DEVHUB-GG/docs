---
description: Documentation for shared.lua Configuration
---

# shared.lua

Settings shared by the client and the server. Long pools (loot props, search items) are trimmed below to a few representative entries — open the file to see the full lists. Never rename the `Shared.*` keys; only change their values.

## <mark style="color:yellow;">**Debug**</mark>

```lua
Shared.Debug = {
    Enabled = false, -- set to false to disable ALL debug prints
    Levels = {
        Info    = true, -- step-by-step flow ("entered X", "branch taken", values)
        Success = true, -- something completed correctly
        Warning = true, -- unexpected but recoverable (missing player, empty list, ...)
        Error   = true, -- something is broken (nil Core, failed export, bad state)
    },
}
```

* **Description**: Master switch for debug prints. Keep `Enabled = false` on a live server.
* **Fields**:
  * **Enabled**: turns all debug prints on or off.
  * **Levels**: fine-grained toggles for each print category (`Info`, `Success`, `Warning`, `Error`).

***

## <mark style="color:yellow;">**MinPolice**</mark>

```lua
Shared.MinPolice = 0
```

* **Description**: Minimum number of police that must be online before a player can join the heist queue. `0` = no requirement.

***

## <mark style="color:yellow;">**SkillTree**</mark>

```lua
Shared.SkillTree = {
    enabled = true,      -- grant XP + apply the laptop perks
    category = 'laptop', -- the laptop's skill tree category (do not change unless you renamed it)
    xpPerJob = 250,      -- base skill XP a player earns for completing a heist
    effects = {
        skill_50 = 'xpBoost',   skill_70 = 'xpBoost',
        skill_51 = 'fastHands', skill_71 = 'fastHands',
        skill_52 = 'stealth',   skill_72 = 'stealth',
    },
}
```

* **Description**: Integration with the **Crime Traders** skill tree in [devhub\_skillTree](https://store.devhub.gg/product/6438994). The Train Heist perks live in the laptop's skill tree (the `['trainheist']` section). Players earn XP for finishing heists and the perks they unlock kick in automatically. Everything quietly no-ops without the add-on.
* **Fields**:
  * **enabled**: turn the XP rewards and perks on.
  * **category**: the laptop's skill tree category (leave as `'laptop'` unless you renamed it).
  * **xpPerJob**: base skill XP awarded for completing a heist.
  * **effects**: maps each laptop skill node UID to a bonus channel:
    * **xpBoost**: more XP per completed heist.
    * **fastHands**: pick up loot / search crates faster.
    * **stealth**: lower chance to alert the police while raiding.

***

## <mark style="color:yellow;">**StartRequirements**</mark>

```lua
Shared.StartRequirements = {
    -- Item example:
    -- { name = "trainheist_laptop", amount = 1, consume = true },

    -- Custom handler example:
    -- {
    --     label = "Crew Membership",
    --     icon = "fas fa-users",
    --     check = function(source) return true end,
    --     action = function(source) end,
    -- },
}
```

* **Description**: Optional conditions a player must meet to **join** the queue. Empty by default (no requirements). Items flagged `consume = true` are removed when the mission actually starts (not when joining).
* **Entry types**:
  * **Item requirement**: `{ name, amount, consume }` — require an item, optionally removing it once the heist begins.
  * **Custom handler**: a `label` + `icon` shown to the player, a `check(source)` returning whether the player qualifies, and an `action(source)` that runs once the mission actually starts.

***

## <mark style="color:yellow;">**PedSettings**</mark>

```lua
Shared.PedSettings = {
    enabled = true,
    peds = {
        {
            model = "g_m_y_lost_01",
            coords = vector4(1400.6552, 2169.9309, 96.9115, 268.2721),
            blip = {
                enabled = false,
                sprite = 1,
                color = 2,
                scale = 0.8,
                label = "Train Heist Contact",
            },
        },
        -- Add more peds here
    },
}
```

* **Description**: The in-world NPC contact(s) players can talk to in order to start the heist or check their queue position (an alternative to the laptop). Set `enabled = false` to rely only on the laptop Crime Traders app.
* **Fields**:
  * **enabled**: spawn the contact NPC at all.
  * **peds**: each NPC's `model`, `coords`, and a `blip` (`enabled`, `sprite`, `color`, `scale`, `label`).

***

## <mark style="color:yellow;">**LaptopConfig**</mark>

```lua
Shared.LaptopConfig = {
    enabled = true,
    traderId = 'trader_2',
    name = 'Train Heist',
    smallDescription = 'Hack the rail network, intercept a cargo train and raid its freight.',
    description = 'Join the queue and wait for your slot. Collect the heist vehicle ...',
    bannerImage = 'https://upload.devhub.gg/.../trainHeist/taskImage.webp',
    images = { '.../taskImage.webp', '.../img_1.webp' },
    link = "https://store.devhub.gg",
    rewards = {
        items = {},
        money = 1000,
        crimePoints = 500,
        xp = 250,
    },
    tasks = {
        ['uid_1'] = { name = 'Join the queue', description = '...', icon = 'fas fa-sign-in-alt', order = 1 },
        -- ... uid_2 through uid_8, one entry per heist step, ordered
    },
}
```

* **Description**: Integration with the [devhub\_laptop](https://devhub.gg/product/7054272) **Crime Traders** app. When `enabled = true`, the heist appears as a Crime Traders task and notifications use the laptop popup; when `enabled = false`, the heist is started only through the NPC and notifications use `Core.Notify`.
* **Fields**:
  * **enabled**: register the job with the laptop's Crime Traders app.
  * **traderId**: the trader category the job appears under.
  * **name** / **smallDescription** / **description**: text shown in the app.
  * **bannerImage** / **images**: artwork shown in the app gallery (replace with your own hosted images if you have them).
  * **link**: store link shown in the app.
  * **rewards**: bonus rewards claimed from the laptop after the heist, **on top of** the dynamic cargo payout — `money`, `crimePoints`, `xp` and optional `items`.
  * **tasks**: the checklist steps shown to the player, each with a `name`, `description`, `icon` and `order` (the `uid_*` keys are the task identifiers; keep them unique).

***

## <mark style="color:yellow;">**LootProps**</mark>

```lua
Shared.LootProps = {
    { model = "hei_prop_heist_emp",         zOffset = 0.0,   price = 2500, size = "big" },
    { model = "sm_prop_smug_crate_s_bones", zOffset = 0.0,   xClamp = 0.3, price = 1800, size = "big" },
    { model = "ex_prop_adv_case_sm",        zOffset = -0.07, price = 3200, size = "small" },
    -- ... more loot props
}
```

* **Description**: The pricing pool for the loot props spawned on the train. The client spawns these props on the carriages and the server prices the delivered cargo on drop-off — the payout is the sum of the prices of everything you bring back.
* **Fields**:
  * **model**: the prop model spawned on the train.
  * **zOffset**: height above the carriage origin where the prop sits (adjust per prop).
  * **xClamp**: max absolute X offset to keep wide props inside the carriage (`nil` = no limit).
  * **price**: the value added to the payout when this prop is delivered.
  * **size**: `"big"` or `"small"` — used to vary how loot is presented.

***

## <mark style="color:yellow;">**SearchLoot**</mark>

```lua
Shared.SearchLoot = {
    { item = "gold_bar",         label = "Gold Bar",         chance = 15 },
    { item = "diamond_ring",     label = "Diamond Ring",     chance = 10 },
    { item = "rolex",            label = "Rolex Watch",      chance = 12 },
    { item = "money_bag",        label = "Money Bag",        chance = 20 },
    { item = "electronic_parts", label = "Electronic Parts", chance = 25 },
    { item = "weapon_parts",     label = "Weapon Parts",     chance = 18 },
}
```

* **Description**: The pool of items a player can find when **searching a crate** on the train (separate from the carried loot props above).
* **Fields**:
  * **item**: the inventory item granted on a successful search.
  * **label**: name shown in the "Found" notification.
  * **chance**: weighting (relative chance) of this item being the one found.
