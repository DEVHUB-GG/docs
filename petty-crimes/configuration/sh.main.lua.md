---
description: Documentation for sh.main.lua Configuration
---

# sh.main.lua

## <mark style="color:yellow;">**Debug**</mark>

```lua
Config.Debug = false
```

* **Description**: Prints detailed trace logs to the client and server console. Keep it `false` on a live server — it is only useful when reporting an issue.

***

## <mark style="color:yellow;">**Editor Command**</mark>

```lua
Config.Command = 'pettyCrimes'
```

* **Description**: The command that opens the in-game editor, where every activity, its rewards, minigames, coords, dispatch settings and the shop are configured. Only admins can open it.
* **Example**: With the value above, an admin types `/pettyCrimes`. The editor is also reachable from the `/admindevhub` menu.

***

## <mark style="color:yellow;">**Activities**</mark>

```lua
Config.Activities = Config.Activities or {}
```

* **Description**: Filled at runtime with the activity settings loaded from the database. **Do not edit it by hand** — anything you write here is replaced by the values saved from the in-game editor.

***

## <mark style="color:yellow;">**Police Dispatch**</mark>

```lua
Config.SendDispatch = function(data)
    -- Set up your own dispatch system here.
end
```

* **Description**: Called on the **server** every time a crime is committed and a police alert should go out. It ships **empty on purpose** — wire it into whatever dispatch system your server runs (ps-dispatch, cd\_dispatch, qs-dispatch, lb-tablet, your own, …).
* **When it fires**: only when the activity's dispatch is **enabled** and its **alert chance** roll has already passed, so you do not need to re-check either.

Each activity carries its own dispatch settings — a code, title, message, priority, the jobs to notify, a map blip, an alert chance and an on/off toggle. They are edited in-game on the activity's **Dispatch** tab and arrive in the `data` table:

* **data.activity** (`string`): activity key, e.g. `'postalBoxTheft'`.
* **data.code** (`string`): dispatch / call code, e.g. `'10-31'`.
* **data.name** (`string`): alert title, e.g. `'Postal Box Theft'`.
* **data.message** (`string`): alert body / description text.
* **data.priority** (`number`): `1` = high, `2` = medium, `3` = low.
* **data.chance** (`number`): the configured alert chance, `0`–`100` (already rolled).
* **data.jobs** (`table`): array of job names to notify, e.g. `{ 'police' }`.
* **data.blip** (`table`): `{ sprite = n, color = n, scale = n, flashDurationMs = n }`.
* **data.coords** (`vector3`): where the crime happened.
* **data.source** (`number`): server id of the offending player.
* **data.playerName** (`string`): the offending player's name (`GetPlayerName`).

**Example** — forwarding the alert to your own dispatch resource:

```lua
Config.SendDispatch = function(data)
    TriggerEvent('my-dispatch:server:alert', {
        code     = data.code,
        title    = data.name,
        message  = data.message,
        priority = data.priority,
        jobs     = data.jobs,
        coords   = data.coords,
        blip     = data.blip,
    })
end
```

{% hint style="info" %}
Leave the function empty if you do not want police alerts at all — nothing else depends on it.
{% endhint %}
