---
description: Documentation for sh.lang.lua — translatable text and localization.
---

# sh.lang.lua

All player-facing text lives in `Config.Lang` as key → value pairs. Translate the system by
editing only the **values** (right side).

## <mark style="color:yellow;">**Lang**</mark>

```lua
Config.Lang = {
    ['active'] = "Active",
    ['expired'] = "Expired",
    ["license_issued"] = "License Issued",
    ["show_license_cooldown"] = "Please wait %{seconds}s before showing another license.",
    ["player_has_no_licenses"] = "%{player} has no licenses",
    -- ... hundreds more keys (NPC dialog, notifications, and Vue UI labels)
}
```

* **Description**: A single table holding every string the system displays — license statuses, notifications, NPC dialog, fake ID mission text, crafting station prompts, server messages, and all Vue/NUI labels (keys prefixed with `vue_`).

### Rules

* **Do not change the keys** (left side) — they are referenced from code.
* **Only change the values** (right side) to translate or reword text.
* **Keep the `%{value}` placeholders** intact. They are replaced at runtime with dynamic data.
  * **Example**: In `["player_has_no_licenses"] = "%{player} has no licenses"`, `%{player}` is replaced with the target player's name.
  * **Example**: In `["show_license_cooldown"] = "Please wait %{seconds}s before showing another license."`, `%{seconds}` is replaced with the remaining cooldown.

{% hint style="info" %}
Keys prefixed with `vue_` are consumed by the NUI (Vue) interface — the MDT, card holder, license creator, fake ID screen, and scan terminal. Translate them the same way as the in-game strings.
{% endhint %}
