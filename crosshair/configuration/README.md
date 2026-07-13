---
description: >-
  How Crosshair Maker is configured: the in-game admin panel, plus the three files
  you may want to edit.
---

# 🛠️ Configuration

Almost nothing about this script is configured in a file. The features, the allowed crosshair shapes and scope styles, the slider limits, the weapon rules, the chat command and the keys all live in the **in-game admin panel** (`/crosshairadmin`, or `/admindevhub` and then **Crosshair**). You press **Save & apply** and it reaches every player live, with no restart.

That leaves three files you may want to edit:

* [`sh.config.lua`](sh.config.lua.md) : who is allowed to open the admin panel, and the command that opens it. This is the only gameplay-relevant config file.
* [`sh.lang.lua`](sh.lang.lua.md) : every player-facing string, for translating the script.
* [`config.js`](config.js.md) : the UI sound volume and number formatting.

{% hint style="info" %}
Your panel settings are saved to `data/admin.json` inside the resource. It is a plain, readable JSON file, so you can back it up, copy it to another server, or version it. An empty file (`{}`) means "no overrides": the config file is in charge. **Reset to config** in the panel empties it back to `{}`, so the file is still there, it just stops overriding anything. Player crosshairs are not deleted by a reset.
{% endhint %}

Colors are covered on their own page:

{% content-ref url="../ui-color-customization.md" %}
[ui-color-customization.md](../ui-color-customization.md)
{% endcontent-ref %}
