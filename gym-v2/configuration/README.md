---
description: >-
  The handful of files you edit by hand, and everything else that lives in the
  in-game creator instead.
---

# 🛠️ Configuration

{% hint style="info" %}
**Most of the configuration is not in a file.** Gyms, machines, minigames, memberships, fatigue, the XP curve, achievements, supplements and the whole business setup are stored in the database and edited **in game** from `/admindevhub`, under **Gym**.

Start with [gym-creator](../gym-creator/ "mention"). Come back here only for the five things below.
{% endhint %}

The files live in the `configs/` folder of the resource and are never encrypted.

| File | What it holds |
| --- | --- |
| [shared.lua](shared.lua.md) | Debug flag, the skill tree switch, and the Discord logging setup. |
| [client.lua](client.lua.md) | Sound volumes and the detection loop tunables. |
| [server.lua](server.lua.md) | The `OpenStash` hook you wire into your inventory. |
| [translation.lua](translation.lua.md) | Every text the player and the admin can see. |
| [skillTree.lua](skilltree.lua.md) | The optional `devhub_skillTree` perks. |
| [businesses.lua](businesses.lua.md) | Name pools for generated delivery companies and workers. |

***

## <mark style="color:yellow;">**The exercise files**</mark>

Exercises are defined in `escrowed/gym/shared/exercises/`, one file per exercise. Twenty-two ship with the script, plus an `example.lua` that documents every available field: the prop model, the animation, the extra props, the rep interval, the fatigue distribution and the optional lifecycle hooks.

You do not need to touch these to run a gym. Their **numbers** (XP, fatigue, max reps, fatigue distribution) are editable in game on the **Exercises** tab of [gym-config.md](../gym-creator/gym-config.md "mention"), which overrides whatever the file says.

Copy `example.lua`, rename it, change the `id`, and the new exercise appears in the creator's picker on the next restart.

***

## <mark style="color:yellow;">**Where the rest of it lives**</mark>

| What | Where it is edited |
| --- | --- |
| Gyms, machines, minigame assignments, removed props, interaction points, memberships, blips | [gym-creator](../gym-creator/ "mention") |
| Fatigue, XP curve, stat rewards, body parts, streaks, achievements, per-exercise numbers | [gym-config.md](../gym-creator/gym-config.md "mention") |
| Supplement items and their boosts | [supplement-items.md](../gym-creator/supplement-items.md "mention") |
| Minigame presets | [minigames-and-presets.md](../gym-creator/minigames-and-presets.md "mention") |
| Business levels, missions, warehouse catalog, offline workers, shop prices | [admin-panel.md](../businesses/admin-panel.md "mention") |
| Interface colors | [ui-color-customization.md](../ui-color-customization.md "mention") |
