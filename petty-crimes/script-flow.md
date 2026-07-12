---
description: >-
  What Petty Crimes offers, how the in-game editor works, and how a player
  commits each of the eight crimes.
---

# 🌊 Script Flow

Petty Crimes turns the ordinary street furniture of Los Santos into low-level crime. There is no job to take and no mission to start — a player simply walks up to a mailbox, a parking meter, a payphone, a parked car or another pedestrian and takes what is there. The loot is then sold to a black-market dealer.

Eight activities ship with the script. Every one of them is enabled, tuned and placed from the **in-game editor**, and every one of them can alert the police.

***

## <mark style="color:yellow;">**The in-game editor**</mark>

Everything except the four files described in [configuration](configuration/) is edited in-game and saved to the database — no server restart, no file editing.

{% stepper %}
{% step %}
### Open it

Log in as an admin and type `/pettyCrimes`, or open the DevHub admin menu with `/admindevhub` and pick Petty Crimes. Players without admin rights cannot open it.
{% endstep %}

{% step %}
### Pick an activity

The sidebar lists the Dashboard, the Map, the Shop and the eight crimes. The Dashboard shows which activities are on, the Map draws every configured location and lets you teleport to it.
{% endstep %}

{% step %}
### Configure it

Each crime page has the same tabs: **General** (cooldowns, target label, animation, prop models), **Minigame**, **Rewards**, **Coords** and **Dispatch**. Toggle **ENABLED** at the top to make the activity live.
{% endstep %}

{% step %}
### Place the coords

On the **Coords** tab you can type coordinates by hand, **IMPORT** one of the ready-made presets from the `json/` folder, or hit **PLACE IN WORLD** — that closes the editor, drops you into the world and lets you walk to a spot and press `Enter` to drop a coordinate. Save and you are back in the editor.
{% endstep %}

{% step %}
### Save

**SAVE CHANGES** writes the config to the database and it applies immediately.
{% endstep %}
{% endstepper %}

### What every activity shares

* **Rewards** — a list of item / money / empty entries with a percentage chance each. The chances should add up to at most 100; whatever is left over is an implicit "found nothing".
* **Cooldowns** — a cooldown on the spot (or vehicle/ped) itself, so the same mailbox cannot be farmed, and a personal cooldown on the player.
* **Minigame** — three crimes use a fixed, purpose-built minigame; the others let you pick from Balance Core, Matrix Align, Glyph Sequence, Relay Switch and Hex Match, each with its own tunables.
* **Dispatch** — a code, title, message, priority, alert chance, the jobs to notify and a map blip. The alert itself is sent by the `Config.SendDispatch` hook in [sh.main.lua](configuration/sh.main.lua.md), which you wire into your own dispatch system.
* **Skill perks** — with [devhub\_skillTree](configuration/sh.skilltree.lua.md) enabled, every crime grants XP and can be sped up, auto-solved or made quieter.

***

## <mark style="color:yellow;">**The eight activities**</mark>

The values below are the defaults the SQL file installs — all of them are editable.

### 📮 Postal Box Theft

The player walks up to a mailbox standing on the map (`prop_postbox_01a`, `prop_postbox_01b`), takes the eye-target option and picks the lock in the **Mailbox Break-in** minigame. Success empties the box: a little cash, stolen letters, a sheet of stamps or a small parcel — and sometimes nothing at all. The box goes on cooldown for 10 minutes afterwards.

### 🅿️ Parking Meter Theft

Same idea on parking meters (`prop_parknmeter_01`, `prop_parknmeter_02`). The **Parking Meter** minigame has the player force the service panel and flip the switches to empty the coin vault. Loot is coins, parking tokens or the whole coin vault. The meter is on cooldown for 5 minutes.

### ☎️ Phone Booth Theft

Payphones (`prop_phonebox_01a`, `prop_phonebox_02`, `prop_phonebox_04`) are cracked in the **Payphone** minigame — pry off the case, break the lock box, enter the code. The reward is the change inside or the entire coin box. 10-minute cooldown per booth.

{% hint style="info" %}
These three crimes need **no coords at all** to work — leave the coord list empty and every prop of those models, anywhere on the map, becomes robbable. Add coords when you want to restrict the crime to specific spots (the `json/` presets already contain the map's mailboxes, meters and phone boxes).
{% endhint %}

### 📻 Car Radio Theft

The only activity with no world target: the player sits in a vehicle and **uses the tool item** (`devhub_pc_car_radio_tools`, bought from the dealer). By default they must be in the driver's seat. The **Car Radio** minigame has them bypass the security, unscrew the unit and pull the screen — success hands over a car radio. The tool is reusable by default, and each vehicle's radio can only be taken once every 10 minutes.

### 📦 Porch Theft

Delivery packages spawn on porches and doorsteps at the configured coords — up to 8 at a time by default, and a stolen one is replaced as soon as a slot frees up.

{% stepper %}
{% step %}
### Steal the package

The player targets a package, a short progress bar runs and the box ends up in their hands.
{% endstep %}

{% step %}
### Carry it away

The carry animation loops while the package is held. `G` drops it — the drop key, the bone it attaches to and the offsets are all configurable.
{% endstep %}

{% step %}
### Open it

Once dropped, the package can be opened. That runs the minigame (Balance Core by default) and only then is the loot rolled: cash, a delivery package or a cardboard parcel.
{% endstep %}
{% endstepper %}

### 🤏 NPC Pickpocket

The player sneaks up **behind** an ambient pedestrian who is standing still or walking slowly, and takes the "Pickpocket" option. The minigame (Balance Core by default) runs while the player quietly trails the ped, so a walking mark can still be robbed. Success hands over pocket change, a worn wallet, cigarettes, gum, spare keys or a bit of cash — a fair share of attempts turn up nothing.

Fail it and the ped catches the player red-handed and turns hostile for about 12 seconds.

{% hint style="info" %}
The **NPC blacklist** on this activity's page keeps the crime off the wrong peds: it can auto-skip pedestrians spawned by other scripts (shop, job and mission NPCs), and you can block individual ped models such as `s_m_y_cop_01`.
{% endhint %}

### 🔫 NPC Mugging

The loud version. The player aims a **firearm** at a pedestrian within 15 metres and holds the aim; the ped gives in and a "Robbing…" progress bar runs. The take is much better than pickpocketing — up to $180, a fat wallet, a smartphone, a gold chain, a luxury watch or a diamond ring. Afterwards the ped flees.

### 🚗 Smash & Grab

Parked cars spawn at the configured spawn points on every restart (only a random share of the points is used, so the street looks different each time). The server rolls loot for each car and, when it has some, a visible prop sits on the passenger seat — a laptop, a handbag, a duffel bag.

The player smashes the passenger window, reaches in, and clears the minigame to grab what is inside. The loot was already decided when the car was stocked, so the minigame is pure skill — win it and the item is yours. A looted car restocks after a few minutes.

***

## <mark style="color:yellow;">**Selling the loot — the black-market dealer**</mark>

Stolen goods are worthless until someone buys them. A dealer ped spawns at one of the configured locations — picked at random on each restart, or fixed if you turn randomization off — and the player talks to him to open the trade dialogue.

* **Sell**: every stolen item has a price, from a $15 letter up to the more valuable jewellery and electronics.
* **Buy**: the tools the crimes need, such as the car radio tools.

The dealer's ped model, name, role, icon, locations and the whole buy/sell list live on the **Shop** page of the in-game editor.

***

## <mark style="color:yellow;">**Police response**</mark>

Every crime carries its own dispatch settings, and the alert only fires if the activity's dispatch is enabled and its alert-chance roll passes. What happens then is up to you — the `Config.SendDispatch` function ships empty so you can forward the call to whatever dispatch resource your server runs.

{% content-ref url="configuration/sh.main.lua.md" %}
[sh.main.lua.md](configuration/sh.main.lua.md)
{% endcontent-ref %}

{% content-ref url="configuration/sh.skilltree.lua.md" %}
[sh.skilltree.lua.md](configuration/sh.skilltree.lua.md)
{% endcontent-ref %}
