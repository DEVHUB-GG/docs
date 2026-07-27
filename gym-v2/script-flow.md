---
description: >-
  What Gym V2 offers, how a player trains, and how the in-game creator fits
  together.
---

# 🌊 Script Flow

Gym V2 turns any building into a working gym. You place the machines, decide which minigame each one runs, and the script handles the rest: reps, per-muscle fatigue, XP, levels, streaks, achievements, supplements, memberships and a leaderboard.

There is no job to take and no mission to start. A player walks up to a machine, targets it, and trains.

***

## <mark style="color:yellow;">**How a player trains**</mark>

{% stepper %}
{% step %}
### Walk up to a machine

Every machine you placed in the creator has a third-eye target on it. The option is named after the exercise, for example **Bench Press**. A machine can carry several exercises, and then it shows one option per exercise.
{% endstep %}

{% step %}
### The set starts

The player is locked into the animation and the rep loop begins. Every rep runs the minigame you assigned to that machine. Pass it and the rep counts, fail it and the set ends.

If you assigned no minigame, reps simply tick by on their own.
{% endstep %}

{% step %}
### Reps build XP and fatigue

Each rep grants XP and adds fatigue to the muscles that exercise works. A bench press hits the chest hard, the arms and shoulders a little. When the main muscle passes the fatigue threshold the player's muscles give out and the set ends.
{% endstep %}

{% step %}
### Stop when you want

`ESC` stops the set. The current rep finishes first, so nothing is lost. The reps done are reported on screen.
{% endstep %}

{% step %}
### Rest

Fatigue drains on its own, per muscle, per minute. A tired muscle blocks the exercises that use it until it has recovered, which is what pushes players to rotate their routine instead of farming one machine.
{% endstep %}
{% endstepper %}

***

## <mark style="color:yellow;">**What the player gets out of it**</mark>

* **Two stats.** Reps that hit the upper body raise **Strength**, reps that hit the lower body and core raise **Agility**. Strength levels grant bonus max health, Agility levels speed up stamina recovery. Both curves and both reward tables are editable in the creator.
* **Streaks.** Train a minimum number of reps a day and the streak grows. Each streak tier adds an XP bonus.
* **Achievements.** Tiered goals for total reps, streak days, supplements used, memberships bought, workouts completed, reps helped, daily rewards claimed, money spent and unique exercises tried.
* **Leaderboard.** Ranks everyone by total reps and by time spent training.
* **Supplements.** Usable items that grant a timed boost: more XP, less fatigue or a higher chance the rep minigame passes itself.
* **Membership.** A gym can require a paid pass, sell several tiers of it and hand members a free item once a day.

***

## <mark style="color:yellow;">**The in-game creator**</mark>

Everything except the handful of files listed in [configuration](configuration/) is edited in game and saved to the database. No file editing, and only the Gym Config page and the Items page ask for a resource restart.

{% stepper %}
{% step %}
### Open it

Log in as an admin and type `/admindevhub`, then pick **Gym** and **Gym Creator**. Players without the `command` ACE permission cannot open it.
{% endstep %}

{% step %}
### Create a zone

A **zone** is one gym: its machines, its removed props, its interaction points, its membership packages and its blip. A server can have as many zones as you like.
{% endstep %}

{% step %}
### Fill the zone

Inside a zone you work through five tabs: **Equipment**, **Removed Props**, **Interaction Points**, **Other** and **Settings**. Placing a machine drops you into the world with a gizmo, you position it, press `ENTER`, and you are back in the editor.
{% endstep %}

{% step %}
### Save

Confirmed placements are written immediately, so a placed prop can never be lost. Everything else waits for the **Save** button in the top right, which is also what pushes the zone out to every player.
{% endstep %}
{% endstepper %}

{% content-ref url="gym-creator/" %}
[gym-creator](gym-creator/)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**Interaction points**</mark>

A machine is not the only thing a player can click. Each zone can hold any number of interaction points, placed as a prop, a ped or bare coordinates, and each point can carry one or more of these:

* **Gym UI**: opens the gym menu for that zone (dashboard, exercises, leaderboard, achievements, membership).
* **Locker**: a reserved slot for your own locker behavior.
* **Cloakroom**: fires an event name you type in, on the client or on the server, so you can hook up any clothing resource.
* **Boss Menu**: opens the business panel. Only offered when the zone is assigned to a business.

{% content-ref url="gym-creator/interaction-points.md" %}
[interaction-points.md](gym-creator/interaction-points.md)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**The minigames**</mark>

Thirteen rep minigames ship with the script. You pick per machine which ones can come up, and a difficulty range for each. Difficulty scales inside a set: the first rep uses the low end, the last rep the high end.

| Minigame | What the player does |
| --- | --- |
| Alternation | Alternates left and right keys in rhythm. |
| Balance | Holds balance under load. |
| Breathing | Controls breathing in and out. |
| Combo | Enters a key combo sequence. |
| Grip | Holds the grip and squeezes. |
| Path Trace | Follows a moving path. |
| Power Zone | Lands the pointer in the power zone. |
| Pump | Keeps pump tempo on push-ups and pull-ups. |
| Reaction | Reacts to a light signal. |
| Reflex | Fast reflex clicks. |
| Rep Rhythm | Keeps rep tempo on beat. |
| Rhythm | Hits on the metronome beat. |
| Target Lock | Locks onto a moving target. |

You can try any of them from the creator without leaving your chair, and you can save reusable sets as **presets**.

{% content-ref url="gym-creator/minigames-and-presets.md" %}
[minigames-and-presets.md](gym-creator/minigames-and-presets.md)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**Global models**</mark>

Placing machines one by one is the precise way. The fast way is **Global Models**: switch a prop model on, and every prop with that model anywhere on the map becomes trainable, with no zone and no membership involved. Useful for the vanilla gym props scattered around Los Santos.

{% content-ref url="gym-creator/global-models.md" %}
[global-models.md](gym-creator/global-models.md)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**Training partners (the spotter)**</mark>

If the zone belongs to a business, an employee of that business can walk up to a player who is mid-set and take the **Help with exercise** option. The employee then mirrors the same minigame, and every rep they pass is handed to the player as a free rep. The employee's own "reps helped" counter goes up, which is what the business pays bonuses on.

***

## <mark style="color:yellow;">**Skill tree perks**</mark>

With [devhub\_skillTree](configuration/skilltree.lua.md) installed, the gym registers its own tree: fatigue builds slower, muscles recover faster, reps can count twice, minigames can auto-pass, and gym XP is boosted. The script runs perfectly without it.

{% content-ref url="configuration/skilltree.lua.md" %}
[skilltree.lua.md](configuration/skilltree.lua.md)
{% endcontent-ref %}
