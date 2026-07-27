---
description: >-
  The admin tool where you build every gym: zones, machines, minigames, items
  and the global gym settings.
---

# 🔨 Gym Creator

This is where 99% of the setup happens. Nothing on this page needs a file edit or a server restart, except the two places that say so out loud.

***

## <mark style="color:yellow;">**Opening it**</mark>

{% stepper %}
{% step %}
### Type `/admindevhub`

The DevHub admin menu opens. It is the same menu every DevHub script registers into.
{% endstep %}

{% step %}
### Click **Gym**

A dumbbell icon. It expands into the gym entries.
{% endstep %}

{% step %}
### Click **Gym Creator**

The creator opens on the **All Zones** page.
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
Nothing happens when you type the command? You are missing the ACE permission. See the bottom of [installation.md](../installation.md "mention").
{% endhint %}

***

## <mark style="color:yellow;">**The top bar**</mark>

Four things live in the top right corner, and you will use all of them:

| Control | What it does |
| --- | --- |
| **TAB Peek** | Hides the interface so you can look at the world without losing your place. A small `TAB Return to editor` hint stays on screen. Press `TAB` again to come back. |
| **Compact / Expand** | Shrinks the whole panel so you can see more of the room behind it. Handy while placing props in a small MLO. |
| **ESC** | Closes the creator. |
| **X** | The same as `ESC`. |

***

## <mark style="color:yellow;">**The sidebar**</mark>

Six pages, grouped:

| Section | Page | What it is for |
| --- | --- | --- |
| ZONES | **All Zones** | The list of gyms. Create, open and delete them. |
| EDITOR | **Zone Editor** | The zone you currently have open. Five tabs. |
| ITEMS | **Supplement Items** | The supplement items and the boost each one gives. |
| GLOBAL | **Global Models** | Make a prop model trainable everywhere on the map. |
| MINIGAMES | **Minigames & Presets** | Try the minigames, and save reusable minigame sets. |
| CONFIG | **Gym Config** | Fatigue, XP curve, body parts, streaks, achievements, per-exercise tuning. |

{% content-ref url="zones.md" %}
[zones.md](zones.md)
{% endcontent-ref %}

{% content-ref url="equipment.md" %}
[equipment.md](equipment.md)
{% endcontent-ref %}

{% content-ref url="removed-props.md" %}
[removed-props.md](removed-props.md)
{% endcontent-ref %}

{% content-ref url="interaction-points.md" %}
[interaction-points.md](interaction-points.md)
{% endcontent-ref %}

{% content-ref url="other.md" %}
[other.md](other.md)
{% endcontent-ref %}

{% content-ref url="settings.md" %}
[settings.md](settings.md)
{% endcontent-ref %}

{% content-ref url="global-models.md" %}
[global-models.md](global-models.md)
{% endcontent-ref %}

{% content-ref url="minigames-and-presets.md" %}
[minigames-and-presets.md](minigames-and-presets.md)
{% endcontent-ref %}

{% content-ref url="supplement-items.md" %}
[supplement-items.md](supplement-items.md)
{% endcontent-ref %}

{% content-ref url="gym-config.md" %}
[gym-config.md](gym-config.md)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**What saves when**</mark>

This is the one rule worth learning before you touch anything.

{% hint style="success" %}
**A prop you placed and confirmed is stored the moment you press `ENTER`.** Even if the game crashes one second later, the machine is in the database. You can never lose a placement.
{% endhint %}

{% hint style="warning" %}
**Everything else waits for Save.** Zone name, membership packages, the blip, removed props, interaction points, ped-clear areas, minigame assignments: they live in memory until you click the green **Save** button in the top right of the Zone Editor.

**Save** is also what pushes the zone out to every player on the server. Until you press it, the changes are yours alone.
{% endhint %}

Two pages behave differently and say so on screen:

* **Gym Config**: saving writes to the database but does **not** live-reload. Restart the resource.
* **Supplement Items**: same, restart the resource after saving.

***

## <mark style="color:yellow;">**The gizmo (placing things in the world)**</mark>

Any time you place a prop, a machine or a ped, the interface closes and the object appears in front of you with a movement gizmo attached. The controls are always the same:

| Key | Action |
| --- | --- |
| **Mouse (left button)** | Grab and drag the object along the gizmo. |
| **T** | Move mode. |
| **R** | Rotate mode. |
| **LEFT ALT** | Snap the object to the floor or the surface below it. |
| **ENTER** | Confirm. The object stays and the creator reopens. |
| **ESC** | Cancel. The object is deleted and nothing is saved. |

The same hint is drawn on screen while the gizmo is active, so you never have to remember it.

{% hint style="info" %}
Walk to roughly the right spot **before** you place something. The object spawns in front of you, so the closer you start, the less dragging you do.
{% endhint %}

***

## <mark style="color:yellow;">**A first gym in ten minutes**</mark>

{% stepper %}
{% step %}
### Stand inside the building you want to turn into a gym
{% endstep %}

{% step %}
### `/admindevhub` and open the Gym Creator
{% endstep %}

{% step %}
### Click **Create Zone**

A zone called `New Zone` appears and opens straight into the editor.
{% endstep %}

{% step %}
### Rename it

Go to the **Settings** tab and put a real name in **Zone Name**, for example `Vespucci Beach`.
{% endstep %}

{% step %}
### Place a few machines

**Equipment** tab, **Add** button, pick an exercise, position it, `ENTER`. Repeat.

See [equipment.md](equipment.md "mention") for the full tour.
{% endstep %}

{% step %}
### Give it a menu point

**Interaction Points** tab, **Add Point**, tick **Gym UI**, place it as a prop or as bare coordinates near the entrance. This is what players click to open the gym menu.
{% endstep %}

{% step %}
### Add a blip

**Settings** tab, **MAP BLIP** section, switch it on, click **My Position** while you are standing at the door, and set a label.
{% endstep %}

{% step %}
### Click **Save**

The green button in the top right. The gym is now live for everyone.
{% endstep %}
{% endstepper %}
