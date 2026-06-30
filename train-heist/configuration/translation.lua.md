---
description: Documentation for translation.lua Configuration
---

# translation.lua

Every player-facing string lives in the `Shared.Lang` table in `configs/translation.lua`. Translate or reword the values here to localise the script. **Only change the text on the right-hand side — never rename the keys** on the left, and keep any `%{...}` placeholders intact (they are filled in at runtime).

```lua
Shared.Lang = {
    -- Heist flow
    ['start_heist'] = 'Start Heist',
    ['waypoint_antenna'] = 'Head to the antenna location to hack the train network.',
    ['waypoint_terminal'] = 'Go to the terminal to enter the access code.',
    ['hack_monitor'] = 'Hack Monitor',
    ['use_terminal'] = 'Use Terminal',
    ['train_arrived'] = 'The train has stopped. Grab the loot!',
    ['payout_received'] = 'Payout received: %{amount}',
    -- ... and so on
}
```

## <mark style="color:yellow;">**Placeholders**</mark>

Some strings contain `%{name}` placeholders that are replaced with live values. Keep them in your translation (you may move them within the sentence). The placeholders in use are:

* `%{amount}` — a money amount or item quantity (e.g. `payout_received`, `npc_requirement_item`, `notify_reward_cash`).
* `%{item}` — an item name (e.g. `found_item`, `notify_reward_item`).
* `%{label}` — a requirement label (e.g. `notify_missing_requirement`).
* `%{count}` — a loaded-item count (e.g. `item_loaded`, `heist_complete`).
* `%{min}` — the minimum items required before delivery (`delivery_need_more`).
* `%{position}` — the player's queue position (`queue_joined`, `queue_current_position`).
* `%{cooldown}` — remaining cooldown in seconds (`queue_cooldown_seconds`).

{% hint style="info" %}
A few strings (such as the carry-drop hint) contain small HTML tags like `<kbd>X</kbd>` or `<br>`. These render in the UI — keep them if you want the same formatting.
{% endhint %}

## <mark style="color:yellow;">**String groups**</mark>

The table is organised into commented sections so you can find a string quickly:

* **Heist flow** — waypoint hints, hack/plant prompts, the stop/loot announcements and the payout message.
* **Vehicle pickup** — the heist-vehicle blip and collection prompts.
* **Looting** — pick-up / search prompts and their notifications.
* **Vehicle / trunk** — opening the trunk and loading/removing cargo.
* **Drop off** — the delivery prompts and the heist-complete message.
* **Cargo HUD** — the on-screen cargo counter and drop-off availability text.
* **Target labels** — the interaction labels on the NPC and objects.
* **NPC dialog** — the Rail Network Contact's name, role and dialog lines.
* **Notifications** — reward and requirement messages.
* **Queue system** — join / position / cooldown / police messages.
