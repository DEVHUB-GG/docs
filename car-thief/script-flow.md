---
description: >-
  The full gameplay flow of Car Thief from the player's perspective — from
  joining the queue to delivering the stolen vehicle.
---

# 🌊 Script Flow

## Script Flow

This page explains the full gameplay flow of **Car Thief** from the player's perspective — from accepting a job to delivering the stolen car.

***

### Overview

Car Thief is a queue-based criminal activity. A buyer wants a specific high-end car lifted from an underground car meet. The player meets a contact, identifies the target vehicle, clones its key, distracts the owner, steals the car and delivers it to the buyer. The flow follows **7 sequential steps**.

```
Join Queue → Meet Contact → Travel to Car Meet → Identify Target → Clone Key → Distract & Steal → Deliver Vehicle
```

***

### Step 1 — Join the Queue

Players can join the task queue through **two methods**:

* **Laptop** — Open the devhub laptop, navigate to the Car Thief app under the Crime Traders category, and click to join the queue.
* **NPC** — Walk up to the Car Meet Contact NPC and select "Start Task" from the dialog.

#### Requirements

* Minimum police count must be met (`Config.MinPolice`).
* The player must not already have a task in progress.
* The player must not be on personal cooldown (`Config.PersonalCooldownTime`).
* Any custom entries in `Config.StartRequirements` (items or custom checks) must pass.

Once in the queue, the player waits for their turn. The interval between tasks is controlled by `Config.MaxTaskTime` (default: 20 minutes).

> Players can check their queue position at any time by talking to the NPC and selecting "Check Queue".

***

### Step 2 — Meet Your Contact

When the player's turn arrives, an **informant blip** appears on the map. The player travels to the informant and talks to them.

* The informant spawns at one of the locations in `Config.Informant.locations`, picked at random.
* They reveal the **name of the target vehicle's owner** — this is the car you must steal.
* If the player doesn't already carry them, the informant hands over the tools listed in `Config.Informant.giveItems` (the **Car Checker** and **Key Cloner**).

After the conversation the car meet location is marked on the player's GPS.

***

### Step 3 — Travel to the Car Meet

A **car meet blip** appears on the map. Drive to the location.

* The script picks a random location from `Config.CarThiefLocations`.
* At the meet, NPC guests (`Config.Peds`) spawn and mingle, parked vehicles (`Config.VehiclesSettings.models`) are placed, and background music (`Config.Music`) plays in range.
* One of the parked cars belongs to the owner the informant named — that is the target.

***

### Step 4 — Identify the Target Vehicle

Use the **Car Checker** item (`Config.CarChecker.itemName`) to bring up the **Vehicle Registry Terminal** on a handheld device.

1. Type a license plate into the terminal and search it, **or** press the plate-focus key (default: `Tab`, `Config.CarChecker.plateFocus.key`) to swing the camera onto the nearest vehicle's plate so you can read it.
2. The terminal returns an owner profile, vehicle details (model, year, VIN, color, class), insurance and registration status, and a **WANTED / CLEAN** flag.
3. Keep checking plates until you find the car registered to the owner the informant named.

> When you scan the correct car, the script confirms it ("This is the car we're looking for") so you know which vehicle to work on next.

***

### Step 5 — Clone the Vehicle Key

With the target identified, use the **Key Cloner** item (`Config.KeyCloner.itemName`) near the **vehicle owner** to copy the key.

* The device shows a **Key Reader** that fills over time — standing closer to the owner reads the key faster (`Config.KeyCloner.initialization.maxRatePerSecond`).
* A **detection meter** rises while you stand too close to the owner (`minDistance`). Back off to let it fall. If the detection meter fills, the owner realises what you're doing and **calls the police**, ending the attempt.
* On success the key code is saved to the device and you're told to go distract the owner.

***

### Step 6 — Distract the Owner & Steal the Car

#### 6a. Distract the owner

Talk to the vehicle owner to lure them away from the car.

* Each time you open the dialog, **3 lines are offered at random**, and at least one of them is always a "blow your cover" option (`fail = true`) — picking it cancels the job and calls the police.
* Choosing a safe line distracts the owner, who walks away for `Config.KeyCloner.distractingPed.distractTime` (default: 60 seconds).

#### 6b. Unlock & steal

While the owner is distracted, approach the car and use the cloned key to unlock it.

* You must be within `Config.KeyCloner.unlockingCar.maxDistance` of the car.
* Unlocking takes `Config.KeyCloner.unlockingCar.duration` (default: 7.5 seconds).
* Get in and drive off. **Getting into the stolen vehicle raises the alarm** and the police are notified (see the `TriggerAlarm` hook).

***

### Step 7 — Deliver the Vehicle

A **return vehicle blip** appears on the map, pointing to the buyer.

1. Drive the stolen car to the drop-off and park it within `Config.ReturningVehicle.parkRadius` of the marked spot.
2. Talk to the buyer NPC and select "Return vehicle" to hand it over.
3. The task is marked complete.

***

### Task Complete

After the vehicle is handed over, the player receives their rewards:

* If the **laptop** integration is enabled, the **Money**, **Crime Points** and **XP** configured in `Config.LaptopConfig.rewards` are collected from the laptop.
* If `Config.Rewards.enabled` is true, an additional direct **cash** and/or **item** payout is handed out immediately on completion (this does not require the laptop).
* If `Config.SkillTree.enabled` is true, the player gains `Config.SkillTree.xpPerJob` XP in the configured skill category.

A **personal cooldown** (`Config.PersonalCooldownTime`, default: 10 minutes) begins before the player can join the queue again.

***

### Flow Diagram

```
┌─────────────────┐
│  Join the Queue  │  (Laptop or NPC)
└────────┬────────┘
         ▼
┌─────────────────┐
│  Meet Contact    │  Informant names the target owner + gives gear
└────────┬────────┘
         ▼
┌─────────────────┐
│ Travel to Meet   │  Follow the car meet blip
└────────┬────────┘
         ▼
┌─────────────────┐
│ Identify Target  │  Car Checker — scan plates for the wanted car
└────────┬────────┘
         ▼
┌─────────────────┐
│   Clone Key      │  Key Cloner near owner — mind the detection meter
└────────┬────────┘
         ▼
┌─────────────────────────────────────────┐
│        Distract Owner & Steal           │
│  ▸ Talk to owner — avoid the fail line   │
│  ▸ Unlock with cloned key (while away)   │
│  ▸ Drive off → alarm raised              │
└────────┬────────────────────────────────┘
         ▼
┌─────────────────┐
│ Deliver Vehicle  │  Park at the buyer + hand over
└────────┬────────┘
         ▼
┌─────────────────┐
│  Task Complete   │  Collect rewards + cooldown begins
└─────────────────┘
```

***

### Required Items Summary

| Item                            | Used For                       | Config Key                |
| ------------------------------- | ------------------------------ | ------------------------- |
| Car Checker (`devhub_car_checker`) | Scanning plates to find the target | `Config.CarChecker.itemName` |
| Key Cloner (`devhub_key_cloner`)   | Cloning the key & unlocking the car | `Config.KeyCloner.itemName`  |

> Both items are handed to the player by the informant in Step 2 if they don't already have them (`Config.Informant.giveItems`).

***

### Things That Raise the Alarm

The police are alerted (via the `TriggerAlarm` hook in `client.lua` / `server.lua`) when:

* The detection meter fills while cloning the key.
* The player picks a "blow your cover" distraction line.
* The owner otherwise catches the player.
* The player gets into the stolen vehicle.

***

### Skill Tree Effects

If `devhub_skillTree` is installed and `Config.SkillTree.enabled` is true, players can unlock the perks mapped in `Config.SkillTree.skills`:

| Skill          | Effect                                          |
| -------------- | ----------------------------------------------- |
| Steady Hands   | The key cloner detection meter fills slower     |
| Quick Hands    | The key cloner reads the key faster             |
| Silver Tongue  | Distracted vehicle owners stay away longer      |
| Master Thief   | The stolen vehicle unlocks faster               |

Players also earn `Config.SkillTree.xpPerJob` XP each time a job is completed.
