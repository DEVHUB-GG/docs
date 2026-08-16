# 🏋️ Gym V2

A full gym system for your city: place every machine yourself with an in-game gizmo, pick which minigames each machine runs, and let players train, level up, buy memberships, take supplements and chase achievements. Almost everything is configured from the in-game creator and saved to the database, so you rarely touch a file and almost never restart the server.

{% hint style="info" %}
[Tebex Store](https://store.devhub.gg/product/7583123)
{% endhint %}

{% hint style="success" %}
**New here? Read these three pages in order:**

1. [installation.md](installation.md "mention") to get it running.
2. [script-flow.md](script-flow.md "mention") to understand what the script actually does.
3. [gym-creator](gym-creator/ "mention") to build your first gym. This is where 99% of the configuration happens.
{% endhint %}

***

## <mark style="color:yellow;">**Everything in this documentation**</mark>

### Getting started

| Page | What it covers |
| --- | --- |
| [installation.md](installation.md) | Resources, items, database, `server.cfg`, admin permission. |
| [script-flow.md](script-flow.md) | How a player trains and what the whole system does. |
| [map-bundles.md](map-bundles.md) | Ready-made gym layouts for five popular MLOs. |

### The Gym Creator

| Page | What it covers |
| --- | --- |
| [gym-creator](gym-creator/) | Opening it, the sidebar, the gizmo, and what saves when. |
| [zones.md](gym-creator/zones.md) | Creating and deleting gyms. |
| [equipment.md](gym-creator/equipment.md) | Placing machines and assigning their minigames. |
| [removed-props.md](gym-creator/removed-props.md) | Hiding the map props that are in the way. |
| [interaction-points.md](gym-creator/interaction-points.md) | Gym menu, locker, cloakroom and boss menu points. |
| [other.md](gym-creator/other.md) | Clearing NPCs out of the gym. |
| [settings.md](gym-creator/settings.md) | Name, memberships, daily reward and the map blip. |
| [global-models.md](gym-creator/global-models.md) | Making a prop model trainable map-wide. |
| [minigames-and-presets.md](gym-creator/minigames-and-presets.md) | Testing minigames and saving reusable sets. |
| [supplement-items.md](gym-creator/supplement-items.md) | The supplement items and their boosts. |
| [gym-config.md](gym-creator/gym-config.md) | Fatigue, XP, body parts, streaks, achievements, exercises. |

### For players

| Page | What it covers |
| --- | --- |
| [player-menu.md](player-menu.md) | Every tab of the gym menu, and how a workout runs. |

### The business module (optional)

| Page | What it covers |
| --- | --- |
| [businesses](businesses/) | What the module is and how a gym becomes a business. |
| [business-panel.md](businesses/business-panel.md) | The eleven pages the owner and staff use. |
| [admin-panel.md](businesses/admin-panel.md) | Creating businesses, assigning zones, server-wide rules. |

### For developers

| Page | What it covers |
| --- | --- |
| [server.lua](configuration/server.lua.md) | The hooks your resources plug into, including the three that wrap a workout set. |
| [exports.md](exports.md) | Granting, revoking and reading memberships from your own code. |

### Files and appearance

| Page | What it covers |
| --- | --- |
| [configuration](configuration/) | The five files you edit by hand, and where the rest lives. |
| [ui-color-customization.md](ui-color-customization.md) | Recoloring the interface. |
| [troubleshooting.md](troubleshooting.md) | The questions people ask most, with the fix for each. |
