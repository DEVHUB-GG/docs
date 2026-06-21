---
description: Migrating from version 1 to 2 should not be difficult in any way.
---

# 2️⃣ Migration from v1 to v2

## <mark style="color:yellow;">What changed in Version 2?</mark>

* **New Folder Structure**: Easier configuration with a `core` folder containing all mechanics and a `modules` folder with files you might want to configure based on your framework.
* **Sound System Customization**: Added the option to change the sound system.
* **Vehicle Keys System Auto-Detection**: The system now automatically detects the vehicle keys system, and allows modification in a easier way.
* **Vehicle Fuel System Auto-Detection**: The system now automatically detects the vehicle fuel system, and allows modification in a easier way..
* **Easier UI Changes**: UI customization has been simplified.

***

## <mark style="color:yellow;">How to migrate from v1 to v2</mark>

Most users will not need to modify their code for this update; simply plug and play. \
Download the latest version from [GitHub](https://github.com/DEVHUB-GG/devhub_lib).

{% hint style="info" %}
If you have modified any files, you can edit them based on the file map below.
{% endhint %}

### File and Folder Mappings

<table><thead><tr><th>v1 Location</th><th width="242">v2 Location</th><th>Notes</th></tr></thead><tbody><tr><td><code>frameworks/</code></td><td><code>modules/frameworks/</code></td><td>No major changes in code.</td></tr><tr><td><code>c.clothing.lua, s.sql.lua</code></td><td><code>modules/systems/</code></td><td>No changes in code.</td></tr><tr><td><code>s.logs.lua</code></td><td><code>modules/systems/</code></td><td>Changed displayed name from in-game to Steam name.</td></tr><tr><td><code>/targets</code></td><td><code>modules/systems/</code></td><td>Minor changes in <code>c.custom.lua</code></td></tr><tr><td><code>c.ui.lua</code></td><td><code>modules/systems/</code></td><td>Added a few new options. </td></tr><tr><td><code>/client, /server, /shared, /tests, c.main.lua, s.main.lua, sh.autoDetect.lua, shared.lua</code></td><td><code>/core/</code></td><td>Use the latest files.</td></tr></tbody></table>

***

## <mark style="color:yellow;">Vehicle keys and fuel</mark>

{% hint style="warning" %}
You no longer have to edit the `Core.SpawnVehicle` function to add your keys or fuel system. The system now detects them automatically.
{% endhint %}

{% content-ref url="vehicle-keys.md" %}
[vehicle-keys.md](vehicle-keys.md)
{% endcontent-ref %}

{% content-ref url="vehicle-fuel.md" %}
[vehicle-fuel.md](vehicle-fuel.md)
{% endcontent-ref %}
