---
description: Documentation for sh.config.lua Configuration
---

# sh.config.lua

This file answers one question: **who may open the admin panel**. Everything else is set up in game. The whole file is the `Config.Admin` table below. Never rename the keys, only change their values.

## <mark style="color:yellow;">**command**</mark>

```lua
Config.Admin = {
    command = 'crosshairadmin',
}
```

* **Description**: The chat command that opens the **admin panel**. It is command-only on purpose, so it can never end up in a player's GTA key bindings.
* **Example**: With the default value an admin types `/crosshairadmin`. The panel also appears as **Crosshair** in devhub\_lib's `/admindevhub` hub, but that hub only opens for a **devhub\_lib** admin: an admin who got in through `ace` or `identifiers` below has to use the command.

{% hint style="warning" %}
Do not confuse this with the **player** command that opens the maker (`/crosshair` by default). That one is set inside the panel, under **General**, not here.
{% endhint %}

***

## <mark style="color:yellow;">**devhub\_libAdmin**</mark>

```lua
Config.Admin = {
    devhub_libAdmin = true,
}
```

* **Description**: When `true`, anyone devhub\_lib considers an admin may open the panel. Leave it on if you use devhub\_lib admins and want nothing else.
* **Example**: Set it to `false` to ignore devhub\_lib's admin check completely and rely only on `ace` and `identifiers`.

***

## <mark style="color:yellow;">**ace**</mark>

```lua
Config.Admin = {
    ace = 'devhub_crosshair.admin',
}
```

* **Description**: An ACE permission that grants access to the panel. Grant it in your `server.cfg`:

```bash
add_ace group.admin devhub_crosshair.admin allow
```

* **Example**: Set it to `''` (an empty string) if you do not want to use ACE permissions at all.

***

## <mark style="color:yellow;">**identifiers**</mark>

```lua
Config.Admin = {
    identifiers = {
        -- 'license:0000000000000000000000000000000000000000',
        -- 'steam:110000100000000',
    },
}
```

* **Description**: People who can **always** open the panel, whatever the two options above say. Steam, license or Discord identifiers, one per line.
* **Example**: Leave it empty if you do not need it. It is useful for a head admin who must keep access even if the ACE group or the devhub\_lib admin list changes.

{% hint style="info" %}
The three checks are combined with OR: devhub\_lib admin, **or** the ACE permission, **or** a listed identifier. A player who passes none of them gets a "You are not allowed to open the crosshair admin panel." notification.
{% endhint %}
