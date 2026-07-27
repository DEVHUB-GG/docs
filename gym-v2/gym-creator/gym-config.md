---
description: >-
  Fatigue, the XP curve, stat rewards, body parts, streaks, achievements and
  per-exercise tuning.
---

# Gym Config

The tuning page. Everything here is server-wide: it applies to every zone, every machine and every player.

{% hint style="warning" %}
**This page does not live-reload.** Saving writes to the `devhub_gym_global` table, and the values are read when the resource starts. Restart the resource after saving, or nothing changes in game.

The one exception is the tutorial toggle at the top, which applies right away.
{% endhint %}

The header shows an **unsaved changes** marker as soon as you edit anything, and one **Save All** button commits the whole page. Most section cards also carry a small **Reset to defaults** button that restores the shipped values for that section only.

Six tabs across the top.

***

## <mark style="color:yellow;">**General**</mark>

### Gym UI Tutorial

A short guided tour of the gym menu, offered to a player the first time they open it. It walks them through the dashboard, the character model, the fatigue breakdown, the exercise catalog and the membership tab, and it even asks them to hover a bar before it lets them continue.

Switch it off if you would rather your players work it out themselves. This toggle applies live, no restart needed.

### Fatigue

| Field | What it does |
| --- | --- |
| **Max Threshold** | The percentage above which a body part counts as too tired. The exercises that use it are blocked until it recovers. Default `95`. |
| **Default Recovery/min** | How much fatigue drains per minute for a body part that has no override of its own. |
| **Global Multiplier** | Scales every fatigue gain in the script. `1.0` is normal, `0.5` makes training twice as forgiving, `2.0` twice as brutal. This is the fastest single dial for tuning difficulty. |

***

## <mark style="color:yellow;">**Stats**</mark>

### XP Curve

| Field | What it does |
| --- | --- |
| **Level Cap** | The highest reachable Strength / Agility level. |
| **Base XP/Level** | XP needed to go from level 1 to level 2. |
| **Curve Multiplier** | How much steeper each level gets: `xpToNext = base * (mult ^ (level - 1))`. |
| **Base XP/Rep** | A multiplier on how much stat XP each rep hands out. |

### Strength Bonuses (per level)

A stair-step table of bonus max health. Click **Add Tier**, then set a **Level** and a **Bonus HP**.

The active reward is the highest tier whose level is at or below the player's current Strength level, so you can skip levels freely: a tier at level 2 and one at level 4 means levels 2 and 3 both use the first tier.

With no tiers configured, players get no health bonus.

### Agility Bonuses (per level)

The same idea, but the value is a **Stamina Regen %** speed-up instead of health.

### Per Body-Part Weights

Two numbers per body part, **Strength** and **Agility**, deciding which stat that muscle feeds.

Each rep splits its XP across the body parts the exercise works. The XP a stat gets is the sum, over every body part, of (that part's weight) multiplied by (the exercise's fatigue share for that part), all multiplied by **Base XP/Rep**. In practice: upper-body parts carry Strength weight, lower-body and core carry Agility weight, and an exercise that hits both feeds both.

***

## <mark style="color:yellow;">**Body Parts**</mark>

The muscle groups themselves: chest, shoulders, back, arms, abs, thighs and calves.

| Field | Editable | Notes |
| --- | --- | --- |
| **ID** | no | The internal key. Exercises refer to it. |
| **Label** | yes | The name players see. |
| **Max Fatigue** | no | The hard cap. |
| **Recovery/min** | yes | How fast this specific muscle drains while idle. Overrides the global default. |

Lower the recovery on the big muscle groups and players are forced to rotate their routine. Raise it and they can hammer the same machine all evening.

The **New** button adds a body part, and **Reset to defaults** restores the canonical seven.

***

## <mark style="color:yellow;">**Streak**</mark>

| Field | What it does |
| --- | --- |
| **Min Reps per Day** | How many reps a player has to do in a day for that day to count toward their streak. |
| **Bonus Tiers** | A list of **Days** and **XP Bonus %**. The highest tier the player qualifies for is the one that applies. |

The defaults run from a 3-day streak at +5% up to a 30-day streak at +30%.

With no tiers, streaks are still tracked and displayed but grant nothing.

***

## <mark style="color:yellow;">**Achievements**</mark>

The full achievement list, sortable, with **Add Achievement** at the top and a trash button on each.

| Field | What it is |
| --- | --- |
| **UID** | A stable identifier, for example `reps_100`. Never reuse one. |
| **Name** | The title players see. |
| **Icon** | A FontAwesome class, for example `fa-solid fa-trophy`. |
| **Description** | One line explaining what to do. |
| **Family** | Groups tiers together, for example `reps`. Achievements in the same family are shown as one ladder. |
| **Tier** | Where in that ladder it sits (bronze, silver, gold and so on). |
| **Type** | What is counted. |
| **Amount** | The target value. |

The nine countable types are: **Total Reps**, **Streak Days**, **Supplements Used**, **Memberships Bought**, **Workouts Completed**, **Reps Helped**, **Daily Rewards**, **Money Spent** and **Unique Exercises**.

***

## <mark style="color:yellow;">**Exercises**</mark>

Per-exercise tuning. This is where an exercise is worth what it is worth, for every machine that uses it, in every zone.

| Field | What it does |
| --- | --- |
| **XP / rep** | XP awarded per completed rep. |
| **Fatigue / rep** | Base fatigue cost per rep, before supplement and membership reductions. |
| **Max reps** | The longest a single set can run. `0` means unlimited, and then only `ESC`, a failed minigame or exhaustion ends it. |
| **Fatigue distribution** | A weight per body part: how much of that fatigue cost each muscle takes. |

On the weights: `1.0` is a full hit, `0.5` is half, `0` means the muscle is not worked at all. **The first body part with a weight above 0 is the one that gates the exercise**, so it is what the "too tired" message names.

{% hint style="info" %}
Nothing here appears? The exercise definition files are not being read at boot. Check that the resource started without errors.
{% endhint %}
