---
description: Documentation for sh.lang.lua Configuration
---

# sh.lang.lua

Every text the player and the admin can see lives in this file under `Config.Lang`. Translate the **values** (right side) into your language and leave the **keys** (left side) untouched.

```lua
Config.Lang = {
    ['need_vehicle'] = "You need to be inside a vehicle.",
    -- …
}
```

{% hint style="warning" %}
Keep the `%{token}` placeholders intact — the script fills them in at runtime (e.g. `%{time}`, `%{key}`, `%{n}`, `%{total}`, `%{model}`, `%{amount}`, `%{name}`). A few values also contain HTML (`<strong>`, `<span class="…">`, `<kbd>`) or GTA colour codes (`~y~`, `~b~`, `~r~`); keep them if you want the same formatting.
{% endhint %}

The keys are grouped by prefix. The sections below explain what each group covers.

***

## <mark style="color:yellow;">**Notifications & targets** — in-world gameplay</mark>

Errors, cooldown messages and the target prompts shown while committing a crime.

```lua
    ['need_vehicle']            = "You need to be inside a vehicle.",
    ['need_driver_seat']        = "You must be in the driver's seat.",
    ['radio_already_taken']     = "This vehicle's radio was already taken. Try again in %{time}.",
    ['no_permission']           = "You don't have permission to do that.",
    ['personal_cooldown_wait']  = "You must wait %{time} before doing this again.",
    ['no_reward']               = "You didn't find anything.",
    ['spot_already_used']       = "This spot was already used. Try again in %{time}.",

    ['porch_steal_package']     = "Steal package",
    ['porch_open_package']      = "Open package",
    ['porch_drop_hint']         = "<kbd>%{key}</kbd> Drop package<br>",
    ['smash_steal_from_car']    = "Steal from car",
    ['pickpocket_target_label'] = "Pickpocket",
    ['pickpocket_caught']       = "They caught you red-handed!",
    ['mugging_robbing']         = "Robbing...",
```

* **Description**: Notifications, eye-target labels and progress-bar captions for the eight activities.
* **Placeholders**: `%{time}` is a formatted cooldown, `%{key}` is the drop key shown in the carry hint.

***

## <mark style="color:yellow;">**Time units**</mark>

```lua
    ['time_min_sec']            = "%{min}min %{sec}sec",
    ['time_min']                = "%{min}min",
    ['time_sec']                = "%{sec}sec",
```

* **Description**: The short duration format used inside every cooldown message (for example `2min 5sec`).

***

## <mark style="color:yellow;">**Skill tree**</mark>

```lua
    ['skill_unlocked']          = "New criminal instinct learned.",
    ['skill_autocomplete']      = "Muscle memory — you crack it without thinking.",
```

* **Description**: Only shown when `Config.SkillTreeEnabled` is on — see [sh.skillTree.lua](sh.skilltree.lua.md).

***

## <mark style="color:yellow;">**`mg_*`** — minigame UI</mark>

Everything drawn inside the minigames: shared frame labels (timer, instructions, abort) plus the per-minigame titles, hints and step captions.

```lua
    ['mg_instructions']         = "INSTRUCTIONS",
    ['mg_time_remaining']       = "TIME REMAINING",
    ['mg_operation_complete']   = "OPERATION COMPLETE",
    ['mg_slots_secured']        = "%{n} of %{total} slots secured",

    ['mg_carradio_title']       = "CAR RADIO",
    ['mg_carradio_unscrew']     = "Unscrew the four screws",
    ['mg_mailbox_pick_lock']    = "Pick the lock",
    ['mg_meter_flip_switches']  = "Flip all the switches down",
    ['mg_relay_hint']           = 'Route the signal from the <span class="text-[#3FCB7E]">source</span> to the active <span class="text-primary">target</span> channel',
```

* **Description**: Covers the four purpose-built minigames (Car Radio, Payphone, Mailbox, Parking Meter) and the five selectable ones (Balance Core, Matrix Align, Glyph Sequence, Relay Switch, Hex Match).
* **Placeholders**: `%{n}` / `%{total}` are progress counters (slots, channels, containers, glyphs).

***

## <mark style="color:yellow;">**`mdt_*`** — in-game editor</mark>

The admin editor opened with the command in [sh.main.lua](sh.main.lua.md): navigation, the activity tabs (General, Minigame, Rewards, Coords, Dispatch), the dispatch alert form, the reward and item pickers, and the shop editor.

```lua
    ['mdt_nav_overview']        = "OVERVIEW",
    ['mdt_tab_rewards']         = "Rewards",
    ['mdt_save_changes']        = "SAVE CHANGES",
    ['mdt_alert_chance_help']   = "Chance an alert actually fires. 100 = always, 0 = never.",
    ['mdt_rewards_sub']         = "Chances are percentages and should sum to at most 100. Any leftover is implicit empty.",
    ['mdt_total_chance']        = "Total chance: %{n} / 100",
```

* **Description**: Admin-facing only — players never see these. Every `*_help` / `*_sub` key is the small grey explanation printed under a field or section header, and every `*_ph` key is an input placeholder.

***

## <mark style="color:yellow;">**`placer_*`** — in-world coord placer</mark>

```lua
    ['placer_title']            = "COORD PLACER",
    ['placer_hint_add']         = "Add coord",
    ['placer_hint_undo']        = "Remove last",
    ['placer_hint_save']        = "Save & return",
    ['placer_empty_hint']       = "Walk to a spot and press Enter to drop a coordinate.",
    ['placer_saved_n']          = "Placed %{n} coord(s)",
```

* **Description**: The overlay shown when an admin leaves the editor to drop coordinates directly in the world (the **PLACE IN WORLD** button on an activity's Coords tab).

***

## <mark style="color:yellow;">**`shop_*`** — black-market dealer</mark>

```lua
    ['shop_npc_name']           = "Dealer",
    ['shop_npc_role']           = "Black Market",
    ['shop_target_label']       = "Talk to dealer",
    ['shop_greeting']           = "What can I do for you?",
    ['shop_opt_buy']            = "Buy items",
    ['shop_bought']             = "Bought items for $%{amount}.",
    ['shop_err_not_enough_cash'] = "You can't afford that.",
```

* **Description**: The dealer's dialogue, the buy/sell menus and every error the trade can return.
* **Placeholders**: `%{amount}` is the total price of the transaction.

{% hint style="info" %}
The dealer's **name, role and icon** can also be overridden per-server on the Shop tab of the in-game editor. The values here are the defaults used when those fields are left empty.
{% endhint %}
