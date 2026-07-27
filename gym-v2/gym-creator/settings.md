---
description: >-
  The zone's name, its membership packages, the free daily reward and the map
  blip.
---

# Settings tab

Four boxes, top to bottom.

***

## <mark style="color:yellow;">**ZONE INFO**</mark>

**Zone Name**: what the gym is called. It shows up on the zone card, in the admin panel and wherever the gym is named to a player. Give it a real name as soon as you create a zone, otherwise you end up with three gyms all called `New Zone`.

***

## <mark style="color:yellow;">**MEMBERSHIP**</mark>

### Membership Required

The toggle at the top right of the box.

* **Off** (the zone card shows **Free Access**): anyone can walk in and train. No pass, no payment, no packages.
* **On** (the card shows **Membership Required**): a player without an active pass is turned away when they try to use a machine, and the gym menu pushes them to the Membership tab.

Everything below only appears when the toggle is on.

### Membership Packages

The passes a player can buy. Click **Add Package** for each tier you want, and fill in three fields per package:

| Field | Meaning |
| --- | --- |
| **Days** | How long the pass lasts. |
| **Price ($)** | What it costs. |
| **Features (comma separated)** | The selling points listed on the card in the buy screen, for example `Access to VIP areas, Free daily protein drink, +10% Buff Bonus`. This is display text only. |

{% hint style="warning" %}
The **Features** text is marketing copy. It does not grant anything by itself. If you promise a VIP area, build the VIP area.
{% endhint %}

A player can hold one pass per gym at a time.

### Daily Reward

An optional free item members can claim once every 24 hours from the Membership tab of the gym menu.

* Switch it on with the toggle next to the label.
* **Item Name**: the spawn name of the item, for example `gym_iron_surge`.
* **Count**: how many of it.

With the toggle off, the box simply reads `This gym has no daily reward.`

{% hint style="info" %}
If the zone belongs to a business, the owner manages packages and the daily reward from the **Memberships** page of the business panel instead, and the reward item has to come from the business warehouse.
{% endhint %}

***

## <mark style="color:yellow;">**MAP BLIP**</mark>

Puts the gym on the map. Switch it on with the toggle, then:

{% stepper %}
{% step %}
### Set the coordinates

Stand at the door and click **My Position**, or type X / Y / Z by hand.
{% endstep %}

{% step %}
### **Blip Label**

The name shown on the map, for example `Vespucci Beach Gym`.
{% endstep %}

{% step %}
### **Blip Sprite (ID)**

`311` is the dumbbell and the default. Any other GTA blip sprite id works, and the field links out to the full sprite list.
{% endstep %}

{% step %}
### **Size** and **Color**

Size is a scale (`0.8` is a good starting point). Color is a dropdown of the 30 GTA blip colors, from White and Red through to Gold and Black.
{% endstep %}

{% step %}
### Press **Save**

The blip appears for every player right away.
{% endstep %}
{% endstepper %}

***

## <mark style="color:yellow;">**SUMMARY**</mark>

A read-only recap of the zone: how many pieces of **Equipment**, how many **Removed Props** and how many **Interaction Points** it holds. Handy as a last check before you save.
