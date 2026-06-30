---
description: >-
  The full gameplay flow of Train Heist from the player's perspective — from
  joining the queue to delivering the looted cargo.
---

# 🌊 Script Flow

## Script Flow

This page explains the full gameplay flow of **Train Heist** from the player's perspective — from joining the queue to delivering the looted cargo.

***

### Overview

Train Heist is a queue-based criminal activity. Only **one** heist runs at a time; everyone else waits in line and is auto-started as soon as the slot frees up. A crew breaches the rail network, forces a cargo train to stop and raids its freight. The flow follows **8 sequential steps**.

```
Join Queue → Collect Vehicle → Hack Antenna → Crack Terminal → Reach Interception → Plant Device → Raid Train → Deliver Cargo
```

***

### Step 1 — Join the Queue

Players can join the heist queue through **two methods**:

* **Laptop** — Open the devhub laptop, navigate to **Train Heist** under the Crime Traders category, and click to join the queue (`Shared.LaptopConfig.enabled`).
* **NPC** — Walk up to the **Rail Network Contact** NPC (`Shared.PedSettings`) and select "Start Heist" from the dialog.

#### Requirements

* Minimum police count must be met (`Shared.MinPolice`).
* No other heist may be in progress (only one runs at a time).
* The player must not be on personal cooldown (`Config.PersonalCooldownTime`).
* Any custom entries in `Shared.StartRequirements` (items or custom checks) must pass.

Once in the queue, the player waits for their turn. A single heist is safety-capped at `Config.MaxTaskTime` (default: 20 minutes), after which the slot is force-freed for the next player.

> Players can check their queue position at any time by talking to the NPC and selecting "Check Queue".

***

### Step 2 — Collect the Heist Vehicle

When the player's turn arrives, a **waypoint to a depot** appears on the map.

* The depot is picked at random from `Config.VehiclePickupLocations`.
* The heist vehicle spawns once the player is close.
* **Getting into the vehicle begins the heist.** Any items in `Shared.StartRequirements` flagged `consume = true` are removed at this point.

***

### Step 3 — Hack the Antenna

A waypoint marks an **antenna** (`Config.AntennaLocations`). Travel there and hack the monitor to breach the rail network.

* Interacting brings up the antenna monitor on a fixed camera.
* Completing the hack **generates an access code** that you'll need at the terminal.

***

### Step 4 — Crack the Terminal

Go to the **access terminal** (`Config.SecondHackLocation`) and enter the code generated during the antenna hack.

* Enter the code on the second monitor; a short progress bar runs (`Config.CodeVerifyDuration`).
* On success the train route is confirmed and the interception point is revealed. An invalid code is rejected so you can try again.

***

### Step 5 — Reach the Interception Point

A waypoint marks the **interception point** (`Config.InterceptionLocations`). Drive there to set up the ambush.

***

### Step 6 — Plant the Device

Plant the device on the tracks to force the train to stop.

* The heist uses one of three scenarios at random (`Config.Scenarios`): **C4**, **Jammer** or **EMP**.
* Planting plays an animation and takes the scenario's `timer` (default: 15 seconds).
* The train spawns far out (`Config.TrainSpawnDistance`) and is forced to stop just short of the interception point (`Config.TrainStopDistance`).

***

### Step 7 — Raid the Train

Once the train stops, raid the carriages. There are two kinds of loot:

* **Pick-up loot** — use the **Pick Up** prompt on a loot prop. Picking up takes `Config.PickupDuration`. The player then **carries** the item (a two-handed box-carry) and must bring it to the vehicle. Press <kbd>X</kbd> to drop it.
* **Searchable crates** — use the **Search Crate** prompt. Searching takes `Config.SearchDuration` and rolls an item from `Shared.SearchLoot`.

To store cargo, **open the vehicle trunk** and **load** the carried item into the trunk grid (powered by [devhub\_grid](https://docs.devhub.gg)). The on-screen cargo HUD shows how many items are loaded and whether the drop-off is unlocked yet.

{% hint style="warning" %}
**Handling loot can tip off the police.** Each pickup, search and load independently rolls a chance to alert the cops (`Config.Police.callChance`), with a per-player quiet window between calls (`Config.Police.cooldownSeconds`). This applies to **anyone** touching the loot, including teammates.
{% endhint %}

You must load at least `Config.MinItemsToDeliver` items (default: 5) before the drop-off can be completed. More cargo means a bigger payout.

***

### Step 8 — Deliver the Cargo

A waypoint marks the **drop-off** (`Config.DropOffLocations`). Drive the loaded vehicle there and complete the delivery.

* The vehicle must be brought close to the drop-off point.
* On completion the heist is finished and rewards are paid out.

***

### Heist Complete

After delivery the player receives:

* The **dynamic cargo payout** — the sum of the prices of all delivered loot props (`Shared.LootProps`), paid as `black_money` or cash depending on `Config.Payout`.
* The **completion rewards** — a random cash amount and/or a random selection of items (`Config.CompletionRewards`), handed out immediately. This does not require the laptop.
* If the **laptop** integration is enabled, the **Money**, **Crime Points** and **XP** configured in `Shared.LaptopConfig.rewards` are collected from the laptop.
* If `Shared.SkillTree.enabled` is true, the player gains `Shared.SkillTree.xpPerJob` XP in the Crime Traders skill tree.

A **personal cooldown** (`Config.PersonalCooldownTime`, default: 10 minutes) begins before the player can join the queue again. The train and its props auto-clean after `Config.TrainAutoDeleteTime`, and the delivered vehicle is removed after `Config.VehicleDeleteDelay`.

***

### Flow Diagram

```
┌──────────────────┐
│   Join the Queue  │  (Laptop or NPC) — one heist at a time
└────────┬─────────┘
         ▼
┌──────────────────┐
│  Collect Vehicle  │  Drive to a random depot, get in
└────────┬─────────┘
         ▼
┌──────────────────┐
│   Hack Antenna    │  Breach the network → access code
└────────┬─────────┘
         ▼
┌──────────────────┐
│  Crack Terminal   │  Enter the code → route confirmed
└────────┬─────────┘
         ▼
┌──────────────────┐
│ Reach Interception│  Follow the waypoint
└────────┬─────────┘
         ▼
┌──────────────────┐
│   Plant Device    │  C4 / Jammer / EMP → train stops
└────────┬─────────┘
         ▼
┌────────────────────────────────────────┐
│              Raid the Train             │
│  ▸ Pick up loot props (carry them)      │
│  ▸ Search crates                        │
│  ▸ Load cargo into the trunk grid       │
│  ▸ Handling loot may alert the police   │
└────────┬───────────────────────────────┘
         ▼
┌──────────────────┐
│  Deliver Cargo    │  Drive to the drop-off + hand over
└────────┬─────────┘
         ▼
┌──────────────────┐
│  Heist Complete   │  Cargo payout + rewards + cooldown
└──────────────────┘
```

***

### Things That Alert the Police

The police are tipped off (via the `TriggerAlarm` hook in `client.lua` / `server.lua`, after a chance roll) when a player:

* **Picks up** a loot prop off the train (`Config.Police.callChance.pickup`).
* **Searches** a crate (`Config.Police.callChance.search`).
* **Loads** an item into the getaway vehicle (`Config.Police.callChance.load`).

A per-player quiet window (`Config.Police.cooldownSeconds`) prevents a flood of calls while looting fast.

***

### Skill Tree Effects

If `devhub_skillTree` is installed and `Shared.SkillTree.enabled` is true, players unlock perks in the laptop's Crime Traders skill tree that apply automatically:

| Channel       | Effect                                          |
| ------------- | ----------------------------------------------- |
| `xpBoost`     | More XP per completed heist                     |
| `fastHands`   | Pick up loot / search crates faster             |
| `stealth`     | Lower chance to alert the police while raiding  |

Players also earn `Shared.SkillTree.xpPerJob` XP each time a heist is completed.

***

### Where to go next

{% content-ref url="configuration/README.md" %}
[configuration](configuration/README.md)
{% endcontent-ref %}

{% content-ref url="exports-and-events.md" %}
[exports-and-events](exports-and-events.md)
{% endcontent-ref %}
