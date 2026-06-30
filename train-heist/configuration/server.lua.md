---
description: Documentation for server.lua Configuration
---

# server.lua

Server-side settings: the queue timers, the cargo payout, completion rewards and the police alerts. Never rename the `Config.*` keys; only change their values.

## <mark style="color:yellow;">**Debug**</mark>

```lua
Config.Debug = true
```

* **Description**: Enables server-side debug prints to help you track down issues. Set to `false` on a live server.

***

## <mark style="color:yellow;">**MaxTaskTime &amp; PersonalCooldownTime**</mark>

```lua
Config.MaxTaskTime = 20
Config.PersonalCooldownTime = 10
```

* **MaxTaskTime**: safety cap, in **minutes**, on a single heist. The slot is force-freed after this so the next queued player can start.
* **PersonalCooldownTime**: cooldown, in **minutes**, applied to a player once their heist is processed, before they can join the queue again.

***

## <mark style="color:yellow;">**Payout**</mark>

```lua
Config.Payout = {
    type = 'item',
    item = 'black_money',
}
```

* **Description**: How the dynamic cargo payout (the sum of the delivered cargo prices from `Shared.LootProps`) is paid on drop-off.
* **Fields**:
  * **type**: `'item'` → paid as an inventory item (the `item` below); `'cash'` → paid as clean cash straight to the player's pocket.
  * **item**: the inventory item used when `type = 'item'` (default `black_money`).

***

## <mark style="color:yellow;">**Police**</mark>

```lua
Config.Police = {
    callChance = {
        pickup = 15, -- ...grabs a loot prop off the train
        search = 20, -- ...searches a crate
        load   = 10, -- ...loads an item into the getaway vehicle
    },
    cooldownSeconds = 20,
}
```

* **Description**: Every time a player handles loot there is a chance the cops get tipped off. This applies to **anyone** touching the loot, even teammates who did not start the heist.
* **Fields**:
  * **callChance**: chance (0–100) the police get alerted each time a player does the action — `pickup`, `search` or `load`.
  * **cooldownSeconds**: quiet window (seconds) after an alert fires before the **same** player can trigger another one. Stops a flood of calls when looting fast. Set to `0` for an independent roll on every action.

***

## <mark style="color:yellow;">**TriggerAlarm (server hook)**</mark>

```lua
function TriggerAlarm(source, coords)
    -- TODO implement your police call logic here (server side)
end
```

* **Description**: Open-function hook called **server-side** whenever the police actually get alerted (after the chance roll). Plug your own dispatch / alarm logic in here (e.g. ps-dispatch, cd_dispatch, etc.). `source` is the player who tipped off the cops and `coords` is where they were. You can instead handle alerts client-side in `configs/client.lua`.

***

## <mark style="color:yellow;">**CompletionRewards**</mark>

```lua
Config.CompletionRewards = {
    cash = {
        enabled = true,
        min = 5000,
        max = 12000,
    },
    items = {
        enabled = true,
        minItems = 1,
        maxItems = 3,
        pool = {
            { name = "gold_bar",         label = "Gold Bar",         min = 1, max = 3 },
            { name = "diamond_ring",     label = "Diamond Ring",     min = 1, max = 2 },
            -- ... more items
        },
    },
}
```

* **Description**: Rewards granted **directly** to the player the moment a heist is finished successfully, **on top of** the dynamic cargo payout calculated from the delivered loot.
* **Fields**:
  * **cash**: a single random cash amount between `min` and `max` (set `enabled = false` to skip).
  * **items**: a random number of **different** items (between `minItems` and `maxItems`), each given in a random quantity between its own `min` and `max`.
    * **pool**: the possible items — each entry's `min`/`max` is the random quantity for that item; `label` is only used for the notification text (falls back to `name`).
