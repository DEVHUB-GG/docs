---
description: >-
  Placing machines, merging several exercises onto one prop, and assigning the
  minigames each machine runs.
---

# Equipment tab

The first tab of the Zone Editor and the one you will spend the most time in. Everything a player can train on is placed here.

At the top you get a counter (`12 placed in this zone`), an **Add** button, a search box and a **Sort** dropdown (Recent, Name A to Z, Name Z to A, Model A to Z, Category).

***

## <mark style="color:yellow;">**Placing your first machine**</mark>

{% stepper %}
{% step %}
### Walk to roughly the right spot

The machine spawns in front of you, so start close.
{% endstep %}

{% step %}
### Click **Add**

The exercise picker opens under the search box, grouped by category: **strength**, **cardio**, **flexibility**.
{% endstep %}

{% step %}
### Find the exercise

Type in the picker's own search box, or scroll. Each tile shows:

* the exercise **name**,
* the **prop model** it uses, in small gray text underneath,
* an **eye** button (see below),
* an orange **`3x`** badge if you already placed that exercise three times in this zone,
* a green **`S/I/E`** badge (the animation has a start, an idle and an end phase) or a blue **`Idle`** badge (one looping animation).
{% endstep %}

{% step %}
### Preview the animation (optional)

Click the **eye** button on the tile. A short looping clip of that exercise plays right there. Move the mouse away from the clip, or click the eye again, and it closes.

The clip is streamed from the internet. Without a connection it simply says it could not load, and nothing else is affected.
{% endstep %}

{% step %}
### Click the tile

The creator closes and the machine appears in front of you with the gizmo attached.
{% endstep %}

{% step %}
### Position it

`Mouse` to drag, `T` for move mode, `R` for rotate mode, `LEFT ALT` to snap it flat to the floor.
{% endstep %}

{% step %}
### Press `ENTER`

The machine is stored **immediately** and the creator reopens with it in the list. Press `ESC` instead and the prop is deleted and nothing is written.
{% endstep %}
{% endstepper %}

{% hint style="success" %}
Repeat from step 2 for every machine. There is no limit, and you can keep the picker open between placements.
{% endhint %}

***

## <mark style="color:yellow;">**The machine row**</mark>

Each placed machine is a row with its icon, name, prop model and animation badge on the left, and a strip of buttons on the right.

| Button | Color | What it does |
| --- | --- | --- |
| Blue chips | blue | The minigames currently assigned. Nothing to click, just a reminder. |
| **Gamepad** | blue | Opens the minigame panel for this machine. |
| **Clone** | purple | Duplicates the machine and drops you straight into the gizmo to position the copy. The fastest way to build a row of five benches. |
| **Pen** | yellow | Opens the inline editor (coordinates, prop model, exercises on the prop). Click again to close it. |
| **Eye** | yellow, turns green | Highlights the machine in the world so you can find it. |
| **Arrow** | blue | Teleports you to it. |
| **Trash** | red | Removes it from the zone. |

***

## <mark style="color:yellow;">**The inline editor (pen button)**</mark>

* **X / Y / Z**: the exact coordinates. Type them if you have them from somewhere else.
* **Prop Model**: swap the model without re-placing the machine.
* **Exercises on this prop**: every exercise that uses this prop model, with a checkbox each. Tick more than one and the machine offers several options when a player targets it. The first ticked one carries the **PRIMARY** badge. Untick them all and the prop stays as pure decoration.
* **Use My Position**: snaps the machine to where you are standing right now.
* **Re-place with Gizmo**: reopens the gizmo on the existing machine so you can nudge it.

***

## <mark style="color:yellow;">**Two exercises on one prop**</mark>

Some props genuinely host more than one exercise, for example a bench that is used for both a press and a row. There are three ways to set that up.

### Merge into an existing machine

{% stepper %}
{% step %}
### Click the machine's name in the list

The row is now selected (its border turns yellow).
{% endstep %}

{% step %}
### Click **Add**

A new row appears at the top of the picker: **Merge into selected: <machine name>**.
{% endstep %}

{% step %}
### Switch the toggle on
{% endstep %}

{% step %}
### Click an exercise that uses the same prop model

Tiles that qualify light up with a **`+ MERGE`** badge. Clicking one adds it as a second option on the existing prop instead of spawning a new one. No gizmo, nothing to position.
{% endstep %}
{% endstepper %}

### Combo tiles

A purple tile with a layers icon means several exercises share one prop, for example `3x COMBO`. Click it to expand, tick the ones you want, then press **Place selected**. One prop is spawned and it carries all of them.

If a machine is selected and merge mode is on, the same button reads **Add to selected** and merges instead.

### The checkbox list

Open the pen editor on a placed machine and tick the extra exercises under **Exercises on this prop**. Same result, useful when the machine is already in place.

***

## <mark style="color:yellow;">**Assigning minigames**</mark>

{% stepper %}
{% step %}
### Click the blue **gamepad** button on the machine's row

The **ASSIGN MINIGAMES** panel opens inside the row.
{% endstep %}

{% step %}
### Apply a preset (the fast way)

If you saved presets on the Minigames page, they appear as blue chips next to **Apply preset:**. Clicking one replaces this machine's whole minigame list.
{% endstep %}

{% step %}
### Or tick them by hand

Every minigame has a checkbox, its name and a one-line description. Tick as many as you want. When the player does a rep, one of the ticked minigames is picked.
{% endstep %}

{% step %}
### Set the difficulty range

Each ticked minigame gets two sliders: the **minimum** and the **maximum** difficulty.

Difficulty ramps up inside a set. The first rep uses the minimum, the last rep uses the maximum, and the reps in between are spread evenly. Set both sliders to the same value for a flat difficulty.
{% endstep %}

{% step %}
### Close the panel and press **Save**

Minigame assignments are part of the zone, so they go live when you save.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Assign no minigame at all and the reps tick by on their own. That is a perfectly valid setup for a beginner-friendly server, or for cardio machines.
{% endhint %}

{% content-ref url="minigames-and-presets.md" %}
[minigames-and-presets.md](minigames-and-presets.md)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**Tuning what an exercise is worth**</mark>

XP per rep, fatigue per rep, the maximum number of reps in a set and which muscles the exercise works are **not** set per machine. They belong to the exercise itself and are edited on the **Exercises** tab of the Gym Config page, once, for every machine that uses it.

{% content-ref url="gym-config.md" %}
[gym-config.md](gym-config.md)
{% endcontent-ref %}
