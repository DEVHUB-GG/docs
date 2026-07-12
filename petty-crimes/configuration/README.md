---
description: >-
  How Petty Crimes is configured — the in-game editor, the config files and the
  coord presets.
---

# 🛠️ Configuration

{% hint style="info" %}
**Most of the configuration is not in a file.** Every activity (its rewards, minigame, cooldowns, animations, coords, dispatch settings) and the black-market shop are stored in the database and edited **in-game** with the `/pettyCrimes` command or from the `/admindevhub` menu. Changes save instantly — no server restart.
{% endhint %}

The files below are the only ones you edit by hand. They live in the `configs/` folder of the resource and are never encrypted.

| File | What it holds |
| --- | --- |
| [sh.main.lua](sh.main.lua.md) | Debug flag, the editor command, and the police dispatch hook. |
| [sh.lang.lua](sh.lang.lua.md) | Every text the player and the editor can see. |
| [sh.skillTree.lua](sh.skilltree.lua.md) | The optional `devhub_skillTree` perks, XP and caps. |
| [config.js](config.js.md) | Number formatting and sound volume for the UI. |

***

## <mark style="color:yellow;">**Coord presets (`json/` folder)**</mark>

The resource ships a `json/` folder with ready-made coordinate lists for the world props the activities use — mailboxes, parking meters, phone boxes and porch doors:

```
json/
  example.json
  porch_doors.json
  prop_parknmeter_01.json
  prop_parknmeter_02.json
  prop_phonebox_01a.json   (…and the other phone box models)
  prop_postbox_01a.json
```

* **Description**: In the editor, open an activity's **Coords** tab and use **IMPORT** to load one of these files instead of placing every point by hand. You can also drop your own file in the folder and it becomes importable.
* **Format**: the same JSON shape Pleb Masters exports — an array of objects with a `Position` (and optionally `Rotation` / `Quaternion`):

```json
[
    {
        "Position": { "x": -75.6, "y": -816.2, "z": 326.2 },
        "Rotation": { "x": 0, "y": 0, "z": 0 },
        "Quaternion": { "x": 0, "y": 0, "z": 0, "w": 1 }
    }
]
```

{% hint style="warning" %}
For **Postal Box**, **Parking Meter** and **Phone Booth** theft, coords are **interaction points for props that already exist on the map** — nothing is spawned there. If no prop of the configured model stands at that position, no target appears. Leave the coord list empty to allow any prop of those models, anywhere on the map.

For **Porch Theft** and **Smash & Grab**, the coords are spawn points instead — packages and parked cars appear at them, so at least one coord is required.
{% endhint %}
