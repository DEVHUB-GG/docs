# 🛡️ Admin Panel

### <mark style="color:yellow;">Opening the panel</mark>

The admin panel is built into the skill tree UI. To access it:

{% stepper %}
{% step %}
#### Make sure your account is recognised as admin

The panel checks `Core.IsPlayerAdmin(source)` from `devhub_lib`. If your framework's admin detection isn't configured in devhub\_lib, the panel will not be accessible.
{% endstep %}

{% step %}
#### Open the skill tree menu

Use your normal command (`/skill`), keybind (`F7`), or item.
{% endstep %}

{% step %}
#### Click the admin button in the top-right corner

The admin button is only rendered when the `isAdmin` callback returns `true` for your `source`.
{% endstep %}
{% endstepper %}

***

### <mark style="color:yellow;">Features</mark>

#### Dashboard

* **Total players** — distinct rows in `dh_skilltree`
* **Total skills unlocked** — sum across all players
* **Total XP earned** — sum across all players
* **Most popular skill** — most-unlocked skill across the server
* **Average level** — mean of `totalLevel` across all players

{% hint style="info" %}
Dashboard stats are cached server-side for **15 minutes** to avoid expensive scans on every open. The UI shows how stale the data is.
{% endhint %}

#### Player Search

Search players by name or server ID. The query hits both online players (live data) and offline players (via SQL on `dh_skilltree.name` and `dh_skilltree.identifier` — the new columns added in v3).

#### Player Details

For any player (online or offline) the panel exposes:

* Their level, XP, points, and unlocked skills per category
* Action buttons to:
  * **Add XP** to a category
  * **Remove XP** from a category
  * **Add points** to a category
  * **Remove points** from a category
  * **Reset skills** in a category (with point refund)
  * **Unlock a specific skill** (skips requirements)

All write actions are logged via the existing log webhooks.

***

### <mark style="color:yellow;">Console commands</mark>

These are registered in `configs/s.main.lua` and enabled by default in v3 (commented out in v2). Permission is `Core.IsPlayerAdmin(source)`.

```
/addXp <playerId> <categoryUid> <amount>
/addPoints <playerId> <categoryUid> <amount>
```

If you want to disable them, comment out the `RegisterCommand` blocks in `configs/s.main.lua`.

***

### <mark style="color:yellow;">Permissions</mark>

Admin status is determined by your devhub\_lib framework adapter — typically the same logic that decides if `/admin` and similar commands work in your other resources. There is no separate ACE / group config for the skill tree itself; if `Core.IsPlayerAdmin(source)` returns `true`, the player can use the panel.

{% hint style="warning" %}
Make sure your framework's `Core.IsPlayerAdmin` adapter is correct. A misconfigured adapter can either lock you out of the panel or grant access to non-admin players.
{% endhint %}

***

### <mark style="color:yellow;">Offline player support</mark>

Offline players are read directly from `dh_skilltree` via SQL. Modifications (add/remove XP, unlock skill, etc.) update the database directly. When the player next connects, they receive the updated state through the normal player-load flow.

***

### <mark style="color:yellow;">Logging</mark>

All admin write actions go through the same Discord webhook log channels configured in `configs/s.main.lua`:

```lua
Config.Logs = {
    ['addXp']       = "webhook_url",
    ['addPoints']   = "webhook_url",
    ['removePoints'] = "webhook_url",
    ['removeXp']    = "webhook_url",
    ['unlockSkill'] = "webhook_url",
    ['skillReset']  = "webhook_url",
    -- ...
}
```
