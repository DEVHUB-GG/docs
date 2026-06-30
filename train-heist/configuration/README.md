# 🛠️ Configuration

Train Heist is configured through four files in the `configs` folder:

* [`shared.lua`](shared.lua.md) — settings used by both sides: queue requirements, the NPC contact, the laptop & skill tree integrations, and the loot/search pools.
* [`client.lua`](client.lua.md) — everything the player experiences: locations (depots, antennas, terminal, interception, drop-off), the device props, the train, the loot props on the carriages, the carry animation, and the client-side police alarm hook.
* [`server.lua`](server.lua.md) — queue timers, the cargo payout, completion rewards, the police alert chances, and the server-side police alarm hook.
* [`translation.lua`](translation.lua.md) — every player-facing string (UI labels and notifications).

{% hint style="warning" %}
Never rename the `Config.*` / `Shared.*` keys — only change their values.
{% endhint %}
