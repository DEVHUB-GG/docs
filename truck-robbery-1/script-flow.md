# 🌊 Script flow

{% embed url="https://www.youtube.com/watch?v=YqYMmWGz_CU" %}

## Script Flow

This page explains the full gameplay flow of **devhub\_crimeScenes** from the player's perspective — from accepting a task to completing disposal.

***

### Overview

Crime Scenes is a queue-based criminal activity where players clean up crime scenes, collect evidence, load it into a vehicle, and dispose of everything. The flow follows **6 sequential steps**.

```
Join Queue → Pick Up Vehicle → Drive to Scene → Clean Up → Load Vehicle → Dispose of Vehicle
```

***

### Step 1 — Join the Queue

Players can join the task queue through **two methods**:

* **Laptop** — Open the devhub laptop, navigate to the Crime Scenes app under the crime traders category, and click to join the queue.
* **NPC** — Walk up to the Crime Scene Handler NPC and select "Start Mission" from the dialog.

#### Requirements

* Minimum police count must be met (`Shared.MinPolice`).
* The player must not already have a task in progress.
* The player must not be on personal cooldown (`Config.PersonalCooldownTime`).

Once in the queue, the player waits for their turn. The interval between tasks is controlled by `Config.MaxTaskTime` (default: 5 minutes).

> Players can check their queue position at any time by talking to the NPC and selecting "Check Queue".

***

### Step 2 — Pick Up the Vehicle

When the player's turn arrives, a **vehicle location** is marked on the map with a blip. The player must travel to this location and pick up the assigned vehicle.

* The vehicle model is selected randomly from `Config.Vehicle.models`.
* Vehicle size (`small`, `medium`, `large`) determines how much evidence spawns at the crime scene.
* Medium and large vehicles require **skill tree unlocks** (`vehicle_size_1`, `vehicle_size_2`).

#### At the Vehicle

* Use the **"Get required items"** target on the vehicle to receive the items you need for the cleanup (body bags, bin bags, cleaning kit).
* If the trunk system is enabled (`Config.Vehicle.targets.toggleTrunk.enable`), use the **"Toggle trunk"** target to open/close the trunk.

***

### Step 3 — Drive to the Crime Scene

After picking up the vehicle, a **crime scene blip** appears on the map. Drive the vehicle to the crime scene location.

The script selects a random location from `Config.CrimeScene.Locations`. Each location has multiple spawn points where objects are placed.

***

### Step 4 — Clean Up the Scene

The crime scene contains several types of evidence that must be dealt with. The amount of each type depends on the vehicle size.

#### 4a. Bodies (Peds)

1. Approach a body on the ground.
2. Use the **"Pack up body"** target — requires a `Body Bag` item.
3. The body is packaged into a body bag prop.
4. Use the **"Lift up packed body"** target to carry it.
5. Carry it to the vehicle and load it into the trunk.

#### 4b. Body Parts

1. Approach body parts on the ground.
2. Use the **"Pack up body parts"** target — requires a `Bin Bag` item.
3. The parts are packaged into a bin bag prop.
4. Use the **"Lift up packed body parts"** target to carry them.
5. Carry them to the vehicle and load into the trunk.

#### 4c. Crates

1. Approach a crate at the scene.
2. Use the **"Lift up object"** target to pick it up.
3. Carry it to the vehicle and load it into the trunk.

> Crates do not require packaging — they can be picked up directly.

#### 4d. Lootable Objects

Lootable objects can be handled in **two ways**:

* **Loot** — Use the **"Loot object"** target to search the object and receive items (e.g., Antique Vase). With the right skill tree upgrade, there's a chance to find unique items.
* **Burn** — Use the **"Burn object"** target to set it on fire and destroy it.

> Lootable objects do **not** need to be loaded into the vehicle.

#### 4e. Blood Stains

1. Equip the **UV Pocket Flashlight** (`WEAPON_POCKETLIGHT`).
2. Aim the UV light at the ground within range (`minDistance: 10.0`) to reveal hidden blood stains.
3. Once a blood stain is visible, use the **"Clean blood"** target — requires a `Cleaning Kit` item.
4. The player performs a mopping animation to clean the stain.

> Blood stains become invisible again after `resetTime` (default: 15 seconds) if not cleaned, so act quickly.

#### Carrying & Placing Objects

* While carrying an object, press the **put-down key** (default: `X` / control `73`) to drop it.
* To load an object into the vehicle trunk, approach the vehicle with the trunk open and use the **"Put in the trunk"** target.
* If the trunk is full, you'll receive an error message.

***

### Step 5 — Load Evidence into Vehicle

After cleaning all elements at the scene, ensure every packaged body, body part, and crate has been loaded into the vehicle trunk.

* Use the **"Put in the trunk"** target while carrying each object near the vehicle.
* Use the **"Remove object from the trunk"** target if you need to take something back out.

> All physical evidence must be loaded before proceeding. Lootable objects and blood stains are handled on-site and are not loaded.

***

### Step 6 — Dispose of the Vehicle

Once the scene is fully cleaned and all evidence is loaded, a **disposal location blip** appears on the map. Drive the vehicle to the disposal point.

There are **three disposal methods** (configured by the server owner — only enabled methods will appear):

#### Burn

1. Drive to the burn disposal location and park in the marked spot.
2. Pick up the **petrol can** from the ground using the target.
3. Use the **"Pour petrol"** target on the vehicle to douse it.
4. **Run away immediately** — the vehicle will explode after a short delay (`timeBeforeExplosion`).

#### Push Off Cliff

1. Drive to the cliff disposal location and park in the marked spot.
2. Exit the vehicle and use the **"Push vehicle"** target.
3. The vehicle is pushed off the cliff, falls, and explodes.

#### Drown

1. Drive to the water disposal location and park in the marked spot.
2. Exit the vehicle and use the **"Push vehicle"** target.
3. The vehicle is pushed into the water and destroyed.

***

### Task Complete

After the vehicle is destroyed, the task is marked as complete. The player receives:

* **Money** reward
* **Crime Points**
* **XP**
* Any **items looted** from lootable objects during the scene

Rewards can be collected from the laptop.

A **personal cooldown** (`Config.PersonalCooldownTime`, default: 15 minutes) begins before the player can join the queue again.

***

### Flow Diagram

```
┌─────────────────┐
│  Join the Queue  │  (Laptop or NPC)
└────────┬────────┘
         ▼
┌─────────────────┐
│ Pick Up Vehicle  │  Travel to blip, get required items
└────────┬────────┘
         ▼
┌─────────────────┐
│ Drive to Scene   │  Follow the crime scene blip
└────────┬────────┘
         ▼
┌─────────────────────────────────────────┐
│          Clean Up the Scene             │
│                                         │
│  ▸ Pack & carry bodies      (Body Bag)  │
│  ▸ Pack & carry body parts  (Bin Bag)   │
│  ▸ Carry crates             (direct)    │
│  ▸ Loot or burn objects                 │
│  ▸ UV light + mop blood  (Cleaning Kit) │
└────────┬────────────────────────────────┘
         ▼
┌─────────────────┐
│ Load into Trunk  │  Put all carried evidence in vehicle
└────────┬────────┘
         ▼
┌─────────────────┐
│ Dispose Vehicle  │  Burn / Push off cliff / Drown
└────────┬────────┘
         ▼
┌─────────────────┐
│  Task Complete   │  Collect rewards from laptop
└─────────────────┘
```

***

### Required Items Summary

| Item                                        | Used For               | Config Key                                              |
| ------------------------------------------- | ---------------------- | ------------------------------------------------------- |
| Body Bag (`devhub_bodybag`)                 | Packaging dead bodies  | `Config.CrimeScene.Objects.peds.requiredItem`           |
| Bin Bag (`devhub_binbag`)                   | Packaging body parts   | `Config.CrimeScene.Objects.bodyParts.requiredItem`      |
| Cleaning Kit (`devhub_cleaningkit`)         | Mopping blood stains   | `Config.CrimeScene.Objects.blood.cleaning.requiredItem` |
| UV Pocket Flashlight (`WEAPON_POCKETLIGHT`) | Revealing hidden blood | `Config.CrimeScene.Objects.blood.uvFlashlightWeapon`    |

> Items are obtained from the vehicle at the start of the task via the "Get required items" target.

***

### Skill Tree Effects

If `devhub_skillTree` is installed, players can unlock upgrades in the `crime_scenes` category:

| Skill                                        | Effect                                  |
| -------------------------------------------- | --------------------------------------- |
| Unlock medium size vehicle                   | Access to medium disposal vehicles      |
| Unlock large size vehicle                    | Access to large disposal vehicles       |
| Faster opening of lootable objects           | Reduces looting time by 30–50%          |
| Faster cleaning of blood and body parts      | Reduces cleaning time by 30–50%         |
| Faster body lifting                          | Reduces body bag lifting time by 30–50% |
| Chance for unique item from lootable objects | 50% chance to find bonus loot           |
| Extended UV blood visibility duration        | Blood stays visible 30% longer under UV |
