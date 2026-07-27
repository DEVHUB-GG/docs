---
description: The supplement items players can take, and what each one boosts.
---

# Supplement Items

A supplement is an inventory item a player uses to get a timed boost while training. This page decides which items count as supplements and what they do.

Eight of them ship with the script and are already configured, so you only come here to change their numbers or to add your own.

***

## <mark style="color:yellow;">**The three boost types**</mark>

| Boost Type | Effect |
| --- | --- |
| **XP Bonus** | More XP from every rep. |
| **Fatigue Reduction** | Less fatigue per rep, so the player can train longer before their muscles give out. |
| **Minigame Chance** | A chance that a rep's minigame passes itself, with no input needed. |

An item can carry more than one boost. `Hardcore Stack`, for example, gives XP and fatigue reduction at the same time.

***

## <mark style="color:yellow;">**Adding a supplement**</mark>

{% stepper %}
{% step %}
### Open **Supplement Items** in the sidebar

The counter at the top says how many are configured.
{% endstep %}

{% step %}
### Click **Add Item**

A `New Item` card appears.
{% endstep %}

{% step %}
### Fill in **Item ID**

The spawn name of the item exactly as your inventory knows it, for example `gym_iron_surge`. The display name is read from your inventory, so it only appears after you save and reopen the page.
{% endstep %}

{% step %}
### Pick the **Boost Type**
{% endstep %}

{% step %}
### Set **Boost %** and **Duration (minutes)**

The card summarizes it back to you as `XP Bonus +20% for 60 min`.
{% endstep %}

{% step %}
### Click **Save**
{% endstep %}

{% step %}
### Restart the resource

Saving writes to the database but does not live-reload. The interface tells you so.
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
The item has to exist in your inventory first, with an image, or it can never be given out or used. See the items step in [installation.md](../installation.md "mention").
{% endhint %}

***

## <mark style="color:yellow;">**What ships by default**</mark>

| Item | Boost |
| --- | --- |
| `gym_iron_surge` | +20% XP for 1 hour |
| `gym_pump_juice` | +20% minigame auto-pass for 1 hour |
| `gym_ripped_recovery` | -25% fatigue for 1 hour |
| `gym_hardcore_stack` | +15% XP and -15% fatigue for 1.5 hours |
| `gym_beast_mode_fuel` | +25% XP and +10% auto-pass for 2 hours |
| `gym_apex_predator` | +20% XP, -20% fatigue and +10% auto-pass for 2 hours |
| `gym_american_mass` | +20% XP and -20% fatigue for 3 hours |
| `gym_king_of_iron` | +30% XP, -25% fatigue and +15% auto-pass for 3 hours |

***

## <mark style="color:yellow;">**Where players get them**</mark>

Active boosts are shown on the gym menu's **Dashboard**, under **Supplement Bonuses**, with the time left on each.

If you run the business module, supplements are stocked in the gym's warehouse and sold from its shop. Without it, hand them out however your server normally sells items.
