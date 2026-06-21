---
hidden: true
---

# devhub\_lib FOR DEVS ONLY

## devhub\_lib — Developer documentation

***

### Getting Started

#### Accessing the Core Object

```lua
-- From an external resource
Core = nil
Citizen.CreateThread( function()
Core = exports['devhub_lib']:GetCoreObject()
    while Core?.Loaded == nil do
        Wait(1000)
        Core = exports['devhub_lib']:GetCoreObject()
    end
end)
```

***

## Core Functions

### Callbacks

Server callbacks allow you to request data from the server and receive a response on the client.

#### Register a Server Callback `SERVER`

```lua
Core.RegisterServerCallback(name, handler)
```

| Parameter | Type       | Description                                                          |
| --------- | ---------- | -------------------------------------------------------------------- |
| `name`    | `string`   | Unique callback name.                                                |
| `handler` | `function` | Handler function. Receives `source`, `cb`, and additional arguments. |

```lua
Core.RegisterServerCallback('myScript:getPlayerData', function(source, cb, playerId)
    local data = { name = GetPlayerName(source), id = playerId }
    cb(data)
end)
```

#### Trigger a Server Callback `CLIENT`

```lua
Core.TriggerServerCallback(name, cb, ...)
```

| Parameter | Type       | Description                       |
| --------- | ---------- | --------------------------------- |
| `name`    | `string`   | The callback name to trigger.     |
| `cb`      | `function` | Function to receive the response. |
| `...`     | `any`      | Additional arguments to pass.     |

```lua
Core.TriggerServerCallback('myScript:getPlayerData', function(data)
    print(data.name)
end, 1)
```

***

### Promises

Wrap asynchronous operations into synchronous-style code. `CLIENT`

```lua
local result = Core.Promise(function(resolve)
    Core.TriggerServerCallback('myScript:getPlayerData', function(data)
        resolve(data)
    end, 1)
end)

print(result.name) -- available immediately after await
```

***

### Notifications & UI

#### Core.Notify `CLIENT`

```lua
Core.Notify("Item collected!", 5000, "success")
-- Types: "info" | "error" | "success" | "warning"
```

#### Core.Notify `SERVER`

```lua
Core.Notify(source, "Welcome back!", 5000, "info")
```

#### Core.ShowStaticMessage `CLIENT`

Persistent message on screen. Pass `false` to hide.

```lua
Core.ShowStaticMessage("Press <kbd>E</kbd> to interact")
Core.ShowStaticMessage(false) -- hide
```

#### Core.ShowControlButtons `CLIENT`

Control button prompts on screen. Pass `false` to hide.

```lua
Core.ShowControlButtons("<kbd>E</kbd> Open  <kbd>G</kbd> Close")
Core.ShowControlButtons(false) -- hide
```

#### Core.ShowProgressbar `CLIENT`

```lua
Core.ShowProgressbar({
    duration = 5000,       -- required, in ms
    text     = "Crafting...",
    placement = "low",     -- "low" | "medium" | "high"
    canStop  = true,       -- allow cancel with X key
}, function(completed)
    if completed then
        print("Done!")
    else
        print("Cancelled!")
    end
end)
```

#### Core.CloseProgressbar `CLIENT`

```lua
Core.CloseProgressbar()
```

#### Core.PopupForm `CLIENT`

Displays a popup form with input fields and/or dropdowns. Returns `status` (bool) and `data` (table).

```lua
local status, data = Core.PopupForm({
    title   = "Transfer Money",
    message = "Enter the amount to transfer",
    yes     = "Confirm",
    no      = "Cancel",
    img     = "https://example.com/image.png", -- optional
    fields  = {
        {
            uid        = "amount",
            field_type = "input",
            type       = "number",
            placeholder = "Amount",
            min = 1,
            max = 10000,
            icon = "fas fa-dollar-sign",
        },
        {
            uid        = "method",
            field_type = "selectDropdown",
            options    = {
                { uid = "cash", text = "Cash", icon = "fas fa-money-bill-wave" },
                { uid = "bank", text = "Bank", icon = "fas fa-credit-card" },
            },
            autoSelectFirst = true,
        },
    },
})

if status then
    print("Amount: " .. data.amount)
    print("Method: " .. data.method)
end
```

#### Core.DecisionPrompt `CLIENT`

Timed decision prompt with keybinds.

```lua
local result = Core.DecisionPrompt({
    title = "Incoming Call",
    text  = "Accept the dispatch?",
    placement = "top-right",
    time = 10000,                    -- ms
    timeRunOutAction = "decline",    -- default action on timeout
}, {
    { key = 246, label = "Y - Accept",  action = "accept" },
    { key = 306, label = "N - Decline", action = "decline" },
})

if result == "accept" then
    print("Accepted!")
end
```

#### Core.ShowRequiredItems `CLIENT`

Shows a required items HUD display.

```lua
Core.ShowRequiredItems({
    title       = "Required Materials",
    checkAmount = true,   -- checks player inventory amounts
    items = {
        { name = "wood",  amount = 5 },
        { name = "steel", amount = 2 },
    },
})
```

#### Core.HideRequiredItems `CLIENT`

```lua
Core.HideRequiredItems()
```

#### Core.CopyClipboard `CLIENT`

```lua
Core.CopyClipboard("Text to copy")
```

***

### NPC Dialog

Interactive dialog system with NPC camera, typing animation, sound, and multiple response types (grid, input, items, payment). `CLIENT`

#### Core.NpcDialog

Returns a result table with `status` (bool) and `data` (table).

```lua
local result = Core.NpcDialog(pedEntity, {
    npc = {
        name = "Shop Keeper",
        role = "Merchant",
        icon = "fas fa-store",
        camera = {                        -- optional
            distance = 0.85,
            height   = 0.65,
        },
        animation = {                     -- optional
            dict = "gestures@m@standing@casual",
            name = "gesture_shrug_hard",
            flag = 2,
        },
    },
    soundFile = "https://example.com/voice.mp3", -- optional
    text = "Welcome! What would you like to do?",

    -- Response type: GRID (choose from options)
    grid = {
        {
            uid   = "buy",
            icon  = "fas fa-shopping-cart",
            title = "Buy Items",
            badge = { text = "NEW", color = "#00ff00" }, -- optional
            span  = 2,                                    -- optional, span 2 columns
        },
        {
            uid   = "sell",
            icon  = "fas fa-dollar-sign",
            title = "Sell Items",
        },
        {
            uid   = "leave",
            icon  = "fas fa-times",
            title = "Goodbye",
        },
    },
})

if result.status then
    print("Player chose: " .. result.data.uid)
end
```

{% hint style="info" %}
You can replace the `grid` key with other response types: `input`, `items`, or `paymentMethod`. Only one response type per dialog.
{% endhint %}

#### Core.CloseNpcDialog `CLIENT`

```lua
Core.CloseNpcDialog()
```

***

### Utility Functions

#### Core.RequestModel `CLIENT`

```lua
local success = Core.RequestModel('adder') -- blocks until loaded
```

#### Core.GetClosestPlayers `CLIENT`

```lua
local players = Core.GetClosestPlayers(10.0)
for _, p in ipairs(players) do
    print(p.id, p.name, p.distance)
    -- p.ped, p.player, p.id, p.name, p.distance
end
```

#### Core.GenerateRandomChar `CLIENT`

```lua
local str = Core.GenerateRandomChar(8) -- "aB3kZ9mQ"
```

#### Core.GenerateString `SHARED`

```lua
local str = Core.GenerateString(7) -- "xK3mZ9q"
```

#### Core.GenerateUid `SERVER` `CLIENT`

```lua
local uid = Core.GenerateUid() -- "1234DHS1713400000"
```

#### Core.GetPlayerPicture `SERVER`

```lua
local url = Core.GetPlayerPicture(source) -- Steam profile picture URL
```

#### Core.GetOnlinePoliceCount `SERVER` `CLIENT`

```lua
local count = Core.GetOnlinePoliceCount()
```

#### Core.DumpTable `SHARED`

```lua
print(Core.DumpTable({ name = "test", value = 123 }))
```

#### Core.GetLengthOfObject `SHARED`

```lua
Core.GetLengthOfObject({ a = 1, b = 2 }) -- 2
```

#### Core.IsObjectEmpty `SHARED`

```lua
Core.IsObjectEmpty({}) -- true
```

#### Core.GetClientTimestamp `CLIENT`

```lua
local ts = Core.GetClientTimestamp() -- Unix timestamp
```

***

### String Utilities

All accessed via `Core.String`. `SHARED`

```lua
Core.String.Capitalize("hello")                     -- "Hello"
Core.String.Uppercase("hello")                      -- "HELLO"
Core.String.Trim("  hello  ")                       -- "hello"
Core.String.Split("a,b,c", ",")                     -- {"a", "b", "c"}
Core.String.Replace("hello world", "world", "lua")  -- "hello lua"
Core.String.Join({"a", "b", "c"}, "-")              -- "a-b-c"
```

***

### Math Utilities

`SHARED`

```lua
Core.Math.Round(3.14159, 2) -- 3.14
Core.Math.Round(3.5)        -- 4
```

***

## Framework

All functions are unified — they work identically regardless of whether the server uses ESX, QBCore, QBOX, or vRP.

### Player Identity

#### Core.GetIdentifier `SERVER`

```lua
local identifier = Core.GetIdentifier(source)
-- ESX: license | QBCore/QBOX: citizenid | vRP: user_id
```

#### Core.GetFullName `SERVER`

```lua
local name = Core.GetFullName(source) -- "John Doe"
```

***

### Money

#### Core.GetCash / Core.AddCash / Core.RemoveCash `SERVER`

```lua
local cash = Core.GetCash(source)
local success = Core.AddCash(source, 500)
local success = Core.RemoveCash(source, 200) -- returns false if insufficient
```

#### Core.GetBank / Core.AddBank / Core.RemoveBank `SERVER`

```lua
local bank = Core.GetBank(source)
local success = Core.AddBank(source, 1000)
local success = Core.RemoveBank(source, 300) -- returns false if insufficient
```

***

### Job

#### Core.GetJob `SERVER`

```lua
local job = Core.GetJob(source)
-- job.name       (string)  "police"
-- job.label      (string)  "Police"
-- job.grade      (number)  3
-- job.gradeLabel (string)  "Sergeant"
-- job.onDuty     (boolean) true
```

#### Core.IsPolice `SERVER`

```lua
local isPolice = Core.IsPolice(source) -- checks for "police", "sheriff", "state"
```

***

### Player Info

#### Core.GetUserInfo `SERVER`

```lua
local info = Core.GetUserInfo(source)
-- info.dateOfBirth  (string)
-- info.sex          (string)
-- info.height       (string)
-- info.nationality  (string)
```

#### Core.GetUserSkin `SERVER`

```lua
local skin = Core.GetUserSkin(source)
-- skin.eyesColor  (number)
-- skin.skinColor  (number)
```

***

## Inventory

All inventory functions are unified across supported inventory systems (ox\_inventory, qb-inventory, etc.).

### Item Management

#### Core.RegisterItem `SERVER`

Registers an item as usable. When a player uses the item, the callback fires.

```lua
Core.RegisterItem('water', function(source, slot, metadata)
    print(source .. " used water from slot " .. slot)
    Core.RemoveItem(source, 'water', 1)
end)
```

#### Core.AddItem `SERVER`

```lua
local success = Core.AddItem(source, 'bread', 3, { quality = 100 })
```

| Parameter  | Type     | Description                  |
| ---------- | -------- | ---------------------------- |
| `source`   | `number` | Player server ID.            |
| `itemName` | `string` | Item spawn name.             |
| `amount`   | `number` | Amount to add (default `1`). |
| `metadata` | `table`  | Optional metadata table.     |

#### Core.RemoveItem `SERVER`

```lua
local success = Core.RemoveItem(source, 'bread', 1)
```

#### Core.GetItemCount `SERVER`

```lua
local count = Core.GetItemCount(source, 'bread')
```

#### Core.CanCarry `SERVER`

```lua
local canCarry = Core.CanCarry(source, 'bread', 5)
```

#### Core.GetAllItems `SERVER`

Returns all items from a player's inventory as an array.

```lua
local items = Core.GetAllItems(source)
for _, item in ipairs(items) do
    -- item.name     (string)
    -- item.amount   (number)
    -- item.metadata (table)
    -- item.label    (string)
    -- item.slot     (number)
end
```

***

### Item Metadata

#### Core.GetItemMetadata `SERVER`

```lua
local meta = Core.GetItemMetadata(source, 3)
-- meta.name     (string)
-- meta.amount   (number)
-- meta.metadata (table)
```

#### Core.SetItemMetadata `SERVER`

```lua
Core.SetItemMetadata(source, 3, { quality = 50, serial = "ABC123" })
```

***

### Item Data

#### Core.GetItemData `SERVER`

Gets global item info (not player-specific).

```lua
local data = Core.GetItemData('bread')
-- data.label  (string) "Bread"
-- data.img    (string) "nui://inventory_script/web/images/bread.png"
```

***

## Target

Unified targeting system supporting `ox_target`, `qb-target`, and others. All functions are `CLIENT`.

### Model Targets

#### Core.AddModelToTarget

```lua
Core.AddModelToTarget('s_m_y_cop_01', {
    name    = "talk_cop",
    event   = "myScript:talkToCop",
    icon    = "fas fa-comment",
    label   = "Talk to Officer",
    handler = function(entity)
        return true -- canInteract condition
    end,
})
```

***

### Coordinate Targets

#### Core.AddCoordsToTarget

Single option:

```lua
Core.AddCoordsToTarget(vector3(100.0, 200.0, 30.0), {
    name   = "shop_zone",
    event  = "myScript:openShop",
    icon   = "fas fa-store",
    label  = "Open Shop",
    radius = 2.0,
})
```

Multiple options (pass an array):

```lua
Core.AddCoordsToTarget(vector3(100.0, 200.0, 30.0), {
    {
        name  = "shop_buy",
        event = "myScript:buy",
        icon  = "fas fa-shopping-cart",
        label = "Buy",
        radius = 2.0,
    },
    {
        name  = "shop_sell",
        event = "myScript:sell",
        icon  = "fas fa-dollar-sign",
        label = "Sell",
        radius = 2.0,
    },
})
```

#### Core.RemoveCoordsFromTarget

```lua
Core.RemoveCoordsFromTarget("shop_zone")
```

***

### Entity Targets

#### Core.AddLocalEntityToTarget

```lua
Core.AddLocalEntityToTarget(entityHandle, {
    name  = "loot_body",
    event = "myScript:loot",
    icon  = "fas fa-box",
    label = "Search",
})
```

#### Core.RemoveLocalEntityFromTarget

```lua
Core.RemoveLocalEntityFromTarget(entityHandle, { "loot_body" })
```

***

### Global Targets

#### Core.AddGlobalVehicleToTarget

```lua
Core.AddGlobalVehicleToTarget({
    name  = "check_trunk",
    event = "myScript:openTrunk",
    icon  = "fas fa-car",
    label = "Open Trunk",
    bones = { "boot" },
})
```

#### Core.RemoveGlobalVehicleFromTarget

```lua
Core.RemoveGlobalVehicleFromTarget({ "check_trunk" })
```

#### Core.AddGlobalPlayerToTarget

```lua
Core.AddGlobalPlayerToTarget({
    name  = "rob_player",
    event = "myScript:robPlayer",
    icon  = "fas fa-hand-holding-usd",
    label = "Rob",
})
```

#### Core.RemoveGlobalPlayerFromTarget

```lua
Core.RemoveGlobalPlayerFromTarget({ "rob_player" })
```

***

## Vehicles

### Spawning & Deleting

#### Core.SpawnVehicle `CLIENT`

Spawns a vehicle with keys and fuel automatically applied.

```lua
Core.SpawnVehicle('adder', vector3(100.0, 200.0, 30.0), 90.0, function(vehicle)
    print("Spawned: " .. vehicle)
end, true, 100.0)
```

| Parameter   | Type       | Description                          |
| ----------- | ---------- | ------------------------------------ |
| `modelName` | `string`   | Vehicle model name or hash.          |
| `coords`    | `vector3`  | Spawn position.                      |
| `heading`   | `number`   | Vehicle heading.                     |
| `cb`        | `function` | Callback receiving vehicle handle.   |
| `networked` | `boolean`  | Network the entity (default `true`). |
| `fuel`      | `number`   | Fuel level 0–100 (default `100`).    |

#### Core.DeleteVehicle `CLIENT`

```lua
local success = Core.DeleteVehicle(vehicle)
```

***

### Vehicle Keys

#### Core.AddVehicleKeys / Core.RemoveVehicleKeys `CLIENT`

```lua
local plate = GetVehicleNumberPlateText(vehicle)
Core.AddVehicleKeys(plate, vehicle)
Core.RemoveVehicleKeys(plate, vehicle)
```

***

### Vehicle Fuel

#### Core.SetVehicleFuel `CLIENT`

```lua
Core.SetVehicleFuel(vehicle, 75.0) -- 0-100
```

***

### Vehicle Helpers

#### Core.GetClosestVehicle `CLIENT`

```lua
local vehicle, distance = Core.GetClosestVehicle()        -- from player
local vehicle, distance = Core.GetClosestVehicle(coords)   -- from coords
```

#### Core.IsSpawnPointClear `CLIENT`

```lua
local isClear = Core.IsSpawnPointClear(vector3(100.0, 200.0, 30.0), 3.0)
```

***

## World

### Peds

#### addPedToCoords (Export) `CLIENT`

Spawns a ped at coordinates with automatic spawn/despawn based on distance (75 units).

```lua
exports['devhub_lib']:addPedToCoords('s_m_y_cop_01', vector4(100.0, 200.0, 30.0, 180.0))
```

***

### Objects

#### Core.SpawnObject `CLIENT`

```lua
local obj = Core.SpawnObject('prop_bench_01a', vector3(100.0, 200.0, 30.0), function(obj)
    print("Spawned object: " .. obj)
end, false) -- isLocal: true = non-networked
```

#### Core.GetObjects `CLIENT`

```lua
local objects = Core.GetObjects() -- all CObject entities in pool
```

***

### Blips

#### Core.AddBlip `CLIENT`

```lua
local blip = Core.AddBlip(vector3(100.0, 200.0, 30.0), 1, 2, 0.8, "My Blip")
```

| Parameter | Type      | Description     |
| --------- | --------- | --------------- |
| `coords`  | `vector3` | Blip position.  |
| `sprite`  | `number`  | Blip sprite ID. |
| `color`   | `number`  | Blip color ID.  |
| `scale`   | `number`  | Blip scale.     |
| `name`    | `string`  | Blip label.     |

#### Core.RemoveBlip `CLIENT`

```lua
Core.RemoveBlip(blip)
```

***

### PolyZone

Built-in poly zone system (based on mkafrin/PolyZone). `CLIENT`

```lua
local zone = Core.CreatePolyZone({
    vector2(100.0, 100.0),
    vector2(110.0, 100.0),
    vector2(110.0, 110.0),
    vector2(100.0, 110.0),
}, {
    name = "my_zone",
    minZ = 28.0,
    maxZ = 35.0,
})
```

Also available via export:

```lua
local zone = exports['devhub_lib']:CreatePolyZone(points, options)
```

***

## Animations & Clothing

### Animations

#### Core.StartAnim `CLIENT`

Full animation with optional prop.

```lua
Core.StartAnim({
    "amb@world_human_gardener_plant@male@base", -- [1] dict
    "base",                                      -- [2] anim
    AnimationOptions = {
        EmoteLoop   = true,
        EmoteMoving = false,
        Prop          = "prop_tool_shovel",     -- optional
        PropBone      = 57005,
        PropPlacement = {0.0, 0.0, 0.0, 0.0, 0.0, 0.0},
    },
})
```

#### Core.StopAnim `CLIENT`

```lua
Core.StopAnim()
```

#### Core.PlayAnim `CLIENT`

Simple one-shot animation.

```lua
Core.PlayAnim("mp_player_inteat@burger", "mp_player_int_eat_burger", 5000, 1)
-- flags: 1 = loop, 51 = moving
```

***

### Clothing

#### Change Clothing `CLIENT`

```lua
TriggerEvent("dh_lib:changeClothing:client", {
    mask  = {45, 0},   -- {drawable, texture}
    pants = {24, 0},
    torso = {15, 0},
    hat   = {12, 0},   -- prop
})
```

#### Reset Clothing `CLIENT`

```lua
TriggerEvent("dh_lib:resetClothing:client")
```

**Variations:** `mask`, `gloves`, `pants`, `backpack`, `boots`, `neckless`, `tshirt`, `vest`, `decals`, `torso` **Props:** `hat`, `glasses`, `ears`, `watch`, `bracelets`

***

## Sound

Requires `xsound` resource. All functions are `CLIENT`.

#### Core.PlaySoundLocally

```lua
Core.PlaySoundLocally("uid_rain", "https://example.com/rain.mp3", 0.5, true)
-- uid, url, volume (0.0-1.0), loop (bool)
```

#### Core.PlaySoundCoords

```lua
Core.PlaySoundCoords("uid_radio", "https://example.com/music.mp3", 0.3, vector3(100.0, 200.0, 30.0), false)
```

#### Core.StopSound

```lua
Core.StopSound("uid_rain")
```

#### Core.FadeIn / Core.FadeOut

```lua
Core.FadeIn("uid_rain", 2000, 0.8)  -- uid, time (ms), target volume
Core.FadeOut("uid_rain", 2000)       -- uid, time (ms)
```

#### Core.ChangeDistance

```lua
Core.ChangeDistance("uid_radio", 50.0)
```

#### Core.SoundExists

```lua
local exists = Core.SoundExists("uid_rain")
```

***

## Database (SQL)

Server-side wrapper around `oxmysql`. All functions are `SERVER`.

#### Execute (multiple rows)

```lua
-- Async
Core.SQL.Execute('SELECT * FROM users WHERE job = ?', { "police" }, function(results)
    print(#results .. " officers found")
end)

-- Sync
local results = Core.SQL.AwaitExecute('SELECT * FROM users WHERE job = ?', { "police" })
```

#### Single (one row)

```lua
local user = Core.SQL.AwaitSingle('SELECT * FROM users WHERE identifier = ? LIMIT 1', { id })
```

#### Insert

```lua
local insertId = Core.SQL.AwaitInsert('INSERT INTO logs (action) VALUES (?)', { "player_join" })
```

#### Update

```lua
local affected = Core.SQL.AwaitUpdate('UPDATE users SET cash = ? WHERE identifier = ?', { 5000, id })
```

***

## Admin & Logging

### Admin

#### Core.IsPlayerAdmin `SERVER`

```lua
local isAdmin = Core.IsPlayerAdmin(source) -- checks ace permission 'command'
```

***

### Logs

#### Core.SendLog `SERVER`

Sends a Discord webhook embed with player info.

```lua
Core.SendLog(source, "https://discord.com/api/webhooks/xxx/yyy", {
    action = "Item Purchased",
    item   = "bread",
    amount = 5,
})
```

#### Core.SendLog `CLIENT`

```lua
Core.SendLog("https://discord.com/api/webhooks/xxx/yyy", {
    action   = "Door Opened",
    location = "Bank Vault",
})
```

***

## Events

#### Player Loaded

```lua
-- Client
RegisterNetEvent("dh_lib:client:playerLoaded", function()
    print("Player loaded")
end)

-- Server
AddEventHandler("dh_lib:server:playerLoaded", function(source)
    print("Player " .. source .. " loaded")
end)
```

#### Player Unloaded

```lua
-- Server
AddEventHandler("dh_lib:server:playerUnloaded", function(source)
    print("Player " .. source .. " disconnected")
end)
```

#### Resource Stop

```lua
-- Server ("dh_all" = entire server shutting down)
AddEventHandler("dh_lib:server:resourceStop", function(resourceName)
    if resourceName == GetCurrentResourceName() or resourceName == "dh_all" then
        -- cleanup logic
    end
end)
```

#### Vehicle Status `CLIENT`

```lua
RegisterNetEvent("dh_lib:client:vehicleStatus", function(vehicle, entered, isDriver)
    if entered then
        print("Entered vehicle, driver: " .. tostring(isDriver))
    else
        print("Exited vehicle")
    end
end)
```

#### Routing Bucket `SERVER`

```lua
TriggerEvent('devhub_lib:server:setPlayerRoutingBucket', bucketId, targetSource)
TriggerEvent('devhub_lib:server:resetPlayerRoutingBucket', targetSource)
```
