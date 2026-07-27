---
description: Hiding the map props that get in the way of your gym.
---

# Removed Props tab

Most gym MLOs come with their own benches, dumbbells and machines baked into the map. They look fine but they are scenery: nothing can be trained on them. This tab deletes them for every player so your own equipment has room.

The counter at the top left says how many props the zone hides.

***

## <mark style="color:yellow;">**Removing props**</mark>

{% stepper %}
{% step %}
### Stand where you can see the props you want gone
{% endstep %}

{% step %}
### Click **Select Props**

The red button at the top right. The creator closes and you drop into selection mode with a control hint on screen.
{% endstep %}

{% step %}
### Aim and click

| Key | Action |
| --- | --- |
| **Mouse** | Look at a prop. |
| **Left mouse button** | Select or deselect the prop you are aiming at. |
| **X** | Deselect the prop you are aiming at. |
| **ENTER** | Confirm the whole selection. |
| **ESC** | Cancel. Nothing changes. |

Selected props are highlighted, so you can see exactly what is going.
{% endstep %}

{% step %}
### Press `ENTER`

The creator reopens and the props are in the list, each with its model hash and its coordinates.
{% endstep %}

{% step %}
### Press **Save**

Removed props are **not** stored on confirm, unlike equipment. They are only written and pushed to all players when you save the zone.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
You can enter selection mode as many times as you like. Each pass adds to the list, it never replaces what is already there.
{% endhint %}

***

## <mark style="color:yellow;">**The list**</mark>

Each entry shows `Hash: 123456789` and the prop's world position, plus two buttons:

| Button | What it does |
| --- | --- |
| **Arrow** | Teleports you to the prop's position. |
| **Rotate back** | Restores that single prop. It comes back for everyone on the next save. |

The orange **Clear** button next to the counter wipes the entire list at once. It asks for confirmation first and tells you how many props it is about to restore.

***

## <mark style="color:yellow;">**Good to know**</mark>

{% hint style="warning" %}
Removal is matched on the prop's model **and** position, so hiding one bench in your gym does not hide every bench of the same model across Los Santos.
{% endhint %}

{% hint style="info" %}
Props are hidden client-side. A player who joins later gets the list when they load in, so latecomers see the same clean room.
{% endhint %}
