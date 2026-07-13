---
description: >-
  Every export and event exposed by Crosshair Maker: what each one does, when it
  fires, its parameters and its return values.
---

# ⚙️ Exports & Events

Crosshair Maker exposes these integration surfaces:

| Surface                | Where                          | Use it to                                                             |
| ---------------------- | ------------------------------ | --------------------------------------------------------------------- |
| **Client exports**     | any client script              | Open the maker, flip the crosshair on or off, read the player's setup. |
| **Events you trigger** | any client **or** server script | Drive one player's crosshair without a return value.                  |
| **Events you listen for** | any client script           | React when the crosshair is toggled or the maker opens and closes.     |

{% hint style="info" %}
The crosshair is a **per-player, client-side** tool: a player's settings live on their own machine. There is no server export and nothing here is authoritative, because there is nothing of value to grant. Use it for presentation (cutscenes, minigames, restricted zones), never as a security gate.
{% endhint %}

Everything obeys the rules a player is already under. A feature you switched off in the admin panel makes the matching **action** a no-op: with **Presets** off, `ApplyPreset` returns `false` and nothing changes, and with **Profiles** off, `LoadProfile` does the same.

{% hint style="warning" %}
The two list getters, `GetPresets` and `GetProfiles`, are the exception: they still return their contents when the feature behind them is switched off. If you build your own preset picker on `GetPresets`, check the switch yourself, or you will offer presets that quietly refuse to apply.
{% endhint %}

{% hint style="warning" %}
A call that arrives **before the player's settings have loaded** is ignored, not queued. It never blocks your thread. In practice this only affects the first second or two after a player spawns.
{% endhint %}

***

## <mark style="color:yellow;">**Client Exports**</mark>

Call these from any client script as `exports.devhub_crosshair:<name>(...)`.

| Export                 | Returns             | Purpose                                        |
| ---------------------- | ------------------- | ---------------------------------------------- |
| `IsEnabled()`          | `boolean`           | Is the custom crosshair showing right now?      |
| `SetEnabled(enabled)`  | `boolean`           | Turn the crosshair on or off.                   |
| `Toggle()`             | `boolean`           | Flip the crosshair.                             |
| `OpenMaker()`          | nothing             | Open the maker for this player.                 |
| `CloseMaker()`         | nothing             | Close the maker.                                |
| `IsMakerOpen()`        | `boolean`           | Is the maker open right now?                    |
| `GetSettings()`        | `table\|nil`        | Read the player's crosshair, as a copy.         |
| `ApplyPreset(key)`     | `boolean`           | Apply one of the built-in presets.              |
| `GetPresets()`         | `table`             | List the presets that exist right now.          |
| `LoadProfile(name)`    | `boolean`           | Load one of the player's saved profiles.        |
| `GetProfiles()`        | `table`             | List the player's saved profile names.          |

### IsEnabled

```lua
local on = exports.devhub_crosshair:IsEnabled()
```

* **What it does**: Tells you whether the player's custom crosshair is currently switched on. Remember that it ships **off**, so this is `false` for a player who has never turned it on.
* **Returns**: `boolean`. It is also `false` when the player's settings have not loaded yet, which is indistinguishable from "the crosshair is off". If that distinction matters to you, wait until the player is fully spawned before asking.

***

### SetEnabled

```lua
local nowOn = exports.devhub_crosshair:SetEnabled(false)
```

* **What it does**: Turns the custom crosshair on or off. The player gets the same notification they would get from the toggle key.
* **When to use**: To hide the crosshair during a cutscene, a camera sequence, or a minigame that draws its own reticle. Always put it back afterwards.
* **Parameters**:
  * `enabled` *(boolean, required)* : `true` to show the crosshair, `false` to hide it. Anything other than `true` is treated as `false`.
* **Returns**: `boolean`, the **new** state. `false` if the player's settings have not loaded yet.

```lua
-- hide it for a cutscene, then give the player back exactly what they had
local was = exports.devhub_crosshair:IsEnabled()
exports.devhub_crosshair:SetEnabled(false)

-- ... your cutscene ...

exports.devhub_crosshair:SetEnabled(was)
```

***

### Toggle

```lua
local nowOn = exports.devhub_crosshair:Toggle()
```

* **What it does**: Flips the crosshair, exactly as the toggle key does.
* **Returns**: `boolean`, the **new** state. `false` if the player's settings have not loaded yet.

***

### OpenMaker

```lua
exports.devhub_crosshair:OpenMaker()
```

* **What it does**: Opens the maker for this player, the same window `/crosshair` opens.
* **When to use**: To put a "Crosshair" button in your own settings menu or pause menu, without giving players a second command.
* **Returns**: nothing. It waits for the interface to be ready in the background, so it returns to you immediately and never blocks your thread.

***

### CloseMaker

```lua
exports.devhub_crosshair:CloseMaker()
```

* **What it does**: Closes the maker if it is open. Does nothing if it is not.
* **Returns**: nothing.

***

### IsMakerOpen

```lua
local open = exports.devhub_crosshair:IsMakerOpen()
```

* **What it does**: Tells you whether the maker window is open, so your own script can stay out of the way (skip a keybind, pause a prompt).
* **Returns**: `boolean`.

***

### GetSettings

```lua
local settings = exports.devhub_crosshair:GetSettings()
```

* **What it does**: Hands you the player's whole crosshair setup: the shape, the colors, the outline, the dot, the scope, the per-weapon rules.
* **Returns**: `table`, a **copy**. Editing it changes nothing: use the other exports to actually change something. **`nil`** if the player's settings have not loaded yet. This is the one export that returns `nil` rather than `false`, so check it before indexing.

***

### ApplyPreset

```lua
local ok = exports.devhub_crosshair:ApplyPreset('devhub')
```

* **What it does**: Applies a built-in preset to the player's crosshair, exactly as clicking it in the maker does. It replaces the design and keeps the player's own on/off state, their saved profiles and their per-weapon rules.
* **When to use**: To hand a new player a sensible crosshair on their first spawn.
* **Parameters**:
  * `key` *(string, required)* : a preset key. `'devhub'` is the built-in house crosshair (DevHub Gold). Use `GetPresets()` for the live list rather than hardcoding the rest.
* **Returns**: `boolean`. **`false`** if there is no such preset, if the player's settings have not loaded, or if you switched the **Presets** feature off in the admin panel.

```lua
-- give a first-time player the house crosshair and switch it on for them
if #exports.devhub_crosshair:GetProfiles() == 0 then
    exports.devhub_crosshair:ApplyPreset('devhub')
    exports.devhub_crosshair:SetEnabled(true)
end
```

***

### GetPresets

```lua
local presets = exports.devhub_crosshair:GetPresets()
```

* **What it does**: Lists the built-in presets, so you do not have to hardcode their keys.
* **Returns**: `table`, an array of `{ key = 'devhub', label = 'DevHub Gold' }`. An **empty table**, never `nil`.

{% hint style="warning" %}
This one ignores the feature switch: it returns the full list even with **Presets** switched off, while `ApplyPreset` returns `false` for every one of them. Check the switch yourself before you show them to a player.
{% endhint %}

***

### LoadProfile

```lua
local ok = exports.devhub_crosshair:LoadProfile('my sniper')
```

* **What it does**: Loads one of the player's own saved profiles and makes it their current crosshair.
* **Parameters**:
  * `name` *(string, required)* : the profile name, exactly as the player saved it.
* **Returns**: `boolean`. **`false`** if the player has no profile by that name, if their settings have not loaded, or if you switched the **Profiles** feature off in the admin panel.

***

### GetProfiles

```lua
local names = exports.devhub_crosshair:GetProfiles()
```

* **What it does**: Lists the names of the player's saved profiles.
* **Returns**: `table`, an array of strings. An **empty table**, never `nil`, for a player who has saved nothing.

***

## <mark style="color:yellow;">**Events You Trigger**</mark>

The same six actions as events, for when you do not need a return value. They are **net events**, so a **server** script can drive one player's crosshair directly, with no glue of its own.

| Event                             | Parameters            | Effect                        |
| --------------------------------- | --------------------- | ----------------------------- |
| `devhub_crosshair:openMaker`      | none                  | Opens the maker.              |
| `devhub_crosshair:closeMaker`     | none                  | Closes the maker.             |
| `devhub_crosshair:toggle`         | none                  | Flips the crosshair.          |
| `devhub_crosshair:setEnabled`     | `enabled` *(boolean)* | Sets the crosshair on or off. |
| `devhub_crosshair:applyPreset`    | `key` *(string)*      | Applies a preset.             |
| `devhub_crosshair:loadProfile`    | `name` *(string)*     | Loads a saved profile.        |

An event gives you no answer back, so it fails quietly: `applyPreset` with an unknown key, or `loadProfile` with the **Profiles** feature switched off, simply does nothing. Use the exports when you need to know whether it worked.

### From a client script

```lua
TriggerEvent('devhub_crosshair:toggle')
TriggerEvent('devhub_crosshair:applyPreset', 'devhub')
```

### From a server script

```lua
TriggerClientEvent('devhub_crosshair:setEnabled', src, false)
TriggerClientEvent('devhub_crosshair:applyPreset', src, 'devhub')
```

***

## <mark style="color:yellow;">**Events You Listen For**</mark>

Client-side. Add a handler in your own resource and react.

| Event                            | Parameters            | Fires when                                                                          |
| -------------------------------- | --------------------- | ----------------------------------------------------------------------------------- |
| `devhub_crosshair:onToggle`      | `enabled` *(boolean)* | The crosshair is switched on or off, **from any source**: the key, the maker, or the API. |
| `devhub_crosshair:onMakerOpen`   | none                  | The maker opens.                                                                     |
| `devhub_crosshair:onMakerClose`  | none                  | The maker closes.                                                                    |

```lua
AddEventHandler('devhub_crosshair:onToggle', function(enabled)
    print('crosshair is now', enabled and 'on' or 'off')
end)
```

{% hint style="info" %}
`onToggle` fires for every route into the master switch, so you only need this one handler. You do not have to also watch the keybind or poll `IsEnabled()`.
{% endhint %}

***

## <mark style="color:yellow;">**Integration Recipes**</mark>

### Hide the crosshair inside a safe zone

```lua
-- client, in your own resource
local hidden = false

AddEventHandler('myzones:enter', function(zone)
    if zone ~= 'safezone' or hidden then return end
    hidden = exports.devhub_crosshair:IsEnabled()   -- remember what they had
    exports.devhub_crosshair:SetEnabled(false)
end)

AddEventHandler('myzones:leave', function(zone)
    if zone ~= 'safezone' then return end
    exports.devhub_crosshair:SetEnabled(hidden)     -- give it back, on or off
    hidden = false
end)
```

### Put a "Crosshair" button in your own settings menu

```lua
-- client, in your own resource
RegisterNUICallback('openCrosshair', function(_, cb)
    exports.devhub_crosshair:OpenMaker()
    cb('ok')
end)

-- and close your own menu while the maker is up
AddEventHandler('devhub_crosshair:onMakerOpen',  function() SetNuiFocus(false, false) end)
AddEventHandler('devhub_crosshair:onMakerClose', function() -- reopen your menu here
end)
```

### Give a brand-new character the house crosshair

```lua
-- server, in your own resource
AddEventHandler('myframework:firstSpawn', function(src)
    TriggerClientEvent('devhub_crosshair:applyPreset', src, 'devhub')
    TriggerClientEvent('devhub_crosshair:setEnabled', src, true)
end)
```

***

## <mark style="color:yellow;">**Related Pages**</mark>

{% content-ref url="configuration/README.md" %}
[README.md](configuration/README.md)
{% endcontent-ref %}

{% content-ref url="script-flow.md" %}
[script-flow.md](script-flow.md)
{% endcontent-ref %}
