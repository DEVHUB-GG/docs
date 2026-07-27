---
description: Documentation for shared.lua Configuration
---

# shared.lua

## <mark style="color:yellow;">**Debug**</mark>

```lua
Shared.Debug = {
    Enabled = false, -- Enable debug prints
    Levels = {
        Info = true,
        Success = true,
        Warning = true,
        Error = true,
    }
}
```

* **Description**: Prints detailed trace logs to the client and server console. Keep `Enabled = false` on a live server, it is only useful when reporting an issue.
* **Levels**: Which categories get printed once debugging is on. Turning `Info` off while leaving `Error` on is a good middle ground when you are chasing one specific problem.

***

## <mark style="color:yellow;">**Skill Tree**</mark>

```lua
Shared.DevhubSkillTreeEnabled = false
```

* **Description**: Turns on the [devhub\_skillTree](https://store.devhub.gg/product/6438994) integration. When `false`, no category is registered, no XP is handed to the tree and no perk affects training.
* **Warning**: Leave this off unless `devhub_skillTree` is installed and started **before** `devhub_gym2`.

{% content-ref url="skilltree.lua.md" %}
[skilltree.lua.md](skilltree.lua.md)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**Discord Logs**</mark>

```lua
Shared.Logs = {
    Webhook = "", -- Discord webhook URL (empty = logs off)
    ...
}
```

* **Description**: Sends an embed to a Discord webhook whenever something worth recording happens. **An empty `Webhook` turns logging off completely**, no matter what the category switches say.
* **Setup**: Create a webhook in your Discord channel settings, paste the URL into `Webhook`, and switch off the categories you do not care about.

The categories are grouped by who caused the action:

### Gym (players)

| Switch | Logs |
| --- | --- |
| `Exercise` | Workouts started and finished. |
| `Supplement` | Supplements taken. |
| `Membership` | Memberships purchased and daily rewards claimed. |
| `Achievement` | Achievement tiers unlocked. |

### Business (players, the business panel)

`Warehouse`, `Employee`, `Bonus`, `Finance`, `Company`, `Permissions`, `OfflineWorkers`, `Shop`.

### GymCreator (admins)

| Switch | Logs |
| --- | --- |
| `Zone` | Zone created, deleted, settings saved. |
| `Equipment` | Equipment placed and removed. |
| `GymConfig` | Gym Config edits. |
| `Minigame` | Minigame presets saved and deleted. |
| `GlobalModels` | Global model overrides saved. |

### BusinessAdmin (admins, the business admin panel)

`BusinessField`, `Employee`, `WarehouseConfig`, `ProgressionConfig`, `OfflineWorkersConfig`, `GlobalSettings`, `InteractionPoint`, `GymZones`.

{% hint style="warning" %}
`Exercise` is the loudest switch by a wide margin: every set a player finishes is one message. On a busy server, turn it off and keep the admin categories on, which is where logs actually help you.
{% endhint %}
