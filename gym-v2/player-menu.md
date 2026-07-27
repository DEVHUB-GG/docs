---
description: What the player sees when they open the gym menu, tab by tab.
---

# 📱 The Player's Gym Menu

The menu opens when a player uses a **Gym UI** interaction point in a zone. Everything in it is read-only progress except the Membership tab, which is where money changes hands.

{% hint style="info" %}
The very first time a player opens it they are asked **"New here? Want a short guided tour?"**. Accepting walks them through every tab in about a minute. You can turn the tour off server-wide on the **General** tab of [gym-config.md](gym-creator/gym-config.md "mention").
{% endhint %}

***

## <mark style="color:yellow;">**The sidebar**</mark>

| Section | Tab |
| --- | --- |
| OVERVIEW | Dashboard, Exercises |
| STATISTICS | Leaderboard, Achievements |
| MEMBERSHIP | Manage Membership |

The MEMBERSHIP section only appears when the gym actually requires a membership. A free gym has four tabs, not five.

***

## <mark style="color:yellow;">**Dashboard**</mark>

The landing tab, and a snapshot of the character.

* **Character Overview**: a live render of the player's body. Muscle groups glow brighter as they fatigue and fade back as they rest.
* **Body Condition**: the **Fatigue** bar, an average across every muscle group. Hovering it breaks the number down per body part, which is how a player finds out that it is specifically their shoulders holding them back.
* **Stats**:
  * **Strength**, levelled by reps that hit upper-body muscle groups. Its reward is **Bonus Max Health**.
  * **Agility**, levelled by reps that hit lower-body and core groups. Its reward is **Stamina Recovery Speed**.
  * Each one shows the current level, the XP bar to the next level, the bonus in effect right now and the next one coming up.
* **Supplement Bonuses**: **Active Boosts**, the sum of every active supplement, membership and streak effect, split into XP, fatigue reduction and easier minigames.
* **Most Active**: the current training streak in days.
* Small previews of the **Leaderboard** and the latest **Achievements**, each with a `View All` link to the full tab.

***

## <mark style="color:yellow;">**Exercises**</mark>

The catalog of everything this gym offers. The counter at the top reads `12 / 18 available`, meaning six exercises are currently blocked.

* **Muscle Fatigue**: a bar per muscle group. Any muscle over the recovery limit turns red, and the note under it spells out the rule: body parts above that percentage cannot be used until they recover.
* **Workout Catalog**: two cards per row. Each card shows the exercise, the muscles it works, its **XP per rep** and its **fatigue per rep**. A card whose main muscle is exhausted is greyed out and says `Chest too tired`.
* **Filters**: tap a muscle-group chip to show only the exercises that train it, sorted with the biggest hitters first. The search box at the top matches names.

{% hint style="success" %}
Nothing is started from this tab. It is a catalog: the player reads it, then walks to the matching machine in the world and targets it.
{% endhint %}

***

## <mark style="color:yellow;">**Leaderboard**</mark>

Two rankings, server-wide:

* **Most Active Players**, by time spent training, shown in days.
* **Total Reps**, by lifetime reps.

Each table shows the position, the player and the number.

***

## <mark style="color:yellow;">**Achievements**</mark>

Every achievement you configured, with a `4 / 30 unlocked` counter and a progress bar on each locked one.

The **Sort** dropdown offers: Tier high to low, Tier low to high, Type / Family, Progress, and Name A to Z.

***

## <mark style="color:yellow;">**Manage Membership**</mark>

Only present when the gym requires a pass.

### With an active pass

A banner reads **Active Membership** with a live countdown in **DAYS / HOURS / MIN**.

### Without one

The banner reads **No Membership** and lists what they are missing, with a **Browse Packages** button underneath.

### Buying a pass

{% stepper %}
{% step %}
### Click **Browse Packages**
{% endstep %}

{% step %}
### Pick a tier

Each card is headed `30 Days Pass - $5000` and lists the features you typed into that package. The tier they already own is marked **Current Plan**, and one they cannot afford or cannot buy is marked **Unavailable**.
{% endstep %}

{% step %}
### Click **Purchase**

A confirmation shows the plan and the price.
{% endstep %}

{% step %}
### Click **Confirm**

The money is taken and the pass is active immediately.
{% endstep %}
{% endstepper %}

### The free daily reward

If the gym has one configured, a **Free Daily Reward** box sits under the packages with a **Claim** button. It resets every 24 hours, and once used it reads **Claimed Today**. Only members can claim it.

***

## <mark style="color:yellow;">**Training itself**</mark>

Nothing in this menu starts a workout. Training happens in the world:

{% stepper %}
{% step %}
### Walk up to a machine and target it

The option is named after the exercise. A machine with several exercises on it shows one option each.
{% endstep %}

{% step %}
### Do the reps

Every rep runs the machine's minigame. Pass it and the rep counts, fail it and the set ends there.
{% endstep %}

{% step %}
### Press `ESC` when you have had enough

The current rep finishes first, then the set ends and the reps are reported.
{% endstep %}
{% endstepper %}

A set also ends on its own when the exercise's **Max reps** is reached or when the worked muscle gives out.
