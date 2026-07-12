---
description: >-
  Every export and open-function hook exposed by the License System — what each
  one does, when it fires, its parameters and its return values.
---

# ⚙️ Exports & Events

The License System exposes four integration surfaces:

| Surface                  | Where                          | Use it to                                                     |
| ------------------------ | ------------------------------ | ------------------------------------------------------------- |
| **Client exports**       | any client script              | Open the UI and read the local player's licenses.             |
| **Server exports**       | any server script              | Read and modify any player's licenses, grants and templates.  |
| **Client open functions**| `configs/c.functions.lua`      | React to (or block) UI actions on the player's machine.       |
| **Server open functions**| `configs/s.functions.lua`      | React to (or block) license actions authoritatively.          |

{% hint style="warning" %}
Client exports read the **local player's own copy** of their licenses. Use them for UI, prompts and menu gating only. Anything that grants money, items, access or job actions must be verified with the **server** export `playerHasLicense`, which is authoritative.
{% endhint %}

***

## <mark style="color:yellow;">**Client Exports**</mark>

Call these from any client script as `exports['devhub_licenses']:<name>(...)`.

| Export                              | Returns                | Purpose                                     |
| ----------------------------------- | ---------------------- | ------------------------------------------- |
| `openMdt(isLaptopApp)`              | –                      | Open the license MDT tablet.                |
| `openCardHolder()`                  | –                      | Open the player's own card holder.          |
| `openCardHolderForPlayer(serverId)` | –                      | Open another player's card holder.          |
| `openFavoriteHand()`                | –                      | Show the favorite licenses in hand.         |
| `getMyLicenses()`                   | `table`                | Licenses the player **owns**.               |
| `getAllMyLicenses()`                | `table`                | All licenses the player **carries**.        |
| `getLicenseById(licenseId)`         | `table\|nil`           | One carried license.                        |
| `hasLicense(templateName)`          | `boolean, string\|nil` | Active license of a template?               |
| `getLicenseCount(templateName)`     | `number`               | How many owned licenses.                    |
| `getMyFavorites()`                  | `table`                | The player's favorited licenses.            |
| `showLicenseTerminal(callback)`     | `table\|nil`           | Open a scan terminal and wait for the scan. |
| `closeLicenseTerminal()`            | –                      | Close the scan terminal.                    |

### openMdt

```lua
exports['devhub_licenses']:openMdt(isLaptopApp)
```

* **What it does**: Opens the main license MDT tablet — the interface used to browse, create and manage licenses.
* **Parameters**:
  * `isLaptopApp` *(boolean, optional)* — pass `true` when opening the MDT from inside `devhub_laptop` so it renders as an app rather than a standalone tablet. Defaults to `false`.
* **Returns**: nothing.
* **Fires**: `OnMdtOpened(isLaptopApp)`.

### openCardHolder

```lua
exports['devhub_licenses']:openCardHolder()
```

* **What it does**: Opens the card holder showing the local player's licenses — the same UI used when the player inspects their own wallet.
* **Returns**: nothing.
* **Fires**: `OnCardHolderOpened(licenseCount)`.

### openCardHolderForPlayer

```lua
exports['devhub_licenses']:openCardHolderForPlayer(targetServerId)
```

* **What it does**: Opens the card holder showing **another player's** licenses. Use it for police searches or any inspection flow driven by your own script instead of the built-in target option.
* **Parameters**:
  * `targetServerId` *(number, required)* — server ID of the player being inspected. The call is ignored if omitted.
* **Returns**: nothing.
* **Fires**: `OnCheckPlayerLicenses` and, on the server, `OnPlayerLicensesChecked` — either can block the request.

### openFavoriteHand

```lua
exports['devhub_licenses']:openFavoriteHand()
```

* **What it does**: Plays the "hold licenses in hand" display with the player's favorited licenses, so nearby players can read them.
* **Returns**: nothing.
* **Fires**: `OnFavoriteHandOpened(favoriteCount)`.

### getMyLicenses

```lua
local licenses = exports['devhub_licenses']:getMyLicenses()
```

* **What it does**: Returns only the licenses the player **originally owns** — licenses issued to them, not ones handed over or confiscated from someone else.
* **Returns**: `table` — map of `licenseId` → license data. Empty table if the player is not loaded yet.

### getAllMyLicenses

```lua
local licenses = exports['devhub_licenses']:getAllMyLicenses()
```

* **What it does**: Returns **everything the player is currently carrying**, including licenses that belong to other people (given, found or confiscated).
* **Returns**: `table` — map of `licenseId` → license data.

{% hint style="info" %}
`getMyLicenses` answers "what is legally mine", `getAllMyLicenses` answers "what is in my pocket right now". Use the first for identity checks, the second for inventory-style UIs.
{% endhint %}

### getLicenseById

```lua
local license = exports['devhub_licenses']:getLicenseById(licenseId)
```

* **What it does**: Fetches a single license the player carries, by its ID.
* **Parameters**:
  * `licenseId` *(string, required)* — the unique license ID, e.g. `"A1B2C3D4"`.
* **Returns**: `table|nil` — the license data, or `nil` if the player is not carrying it.

### hasLicense

```lua
local hasIt, licenseId = exports['devhub_licenses']:hasLicense(templateName)
```

* **What it does**: Checks whether the player holds an **active** license of the given template that they **own**. A suspended, revoked, lost or expired license returns `false`, and so does someone else's card the player happens to be carrying.
* **When to use**: Gating menus, prompts and target options client-side — for example hiding "Drive a taxi" from a player without a taxi permit.
* **Parameters**:
  * `templateName` *(string, required)* — the template's unique name, e.g. `"driving_license"`.
* **Returns**: `boolean, string|nil` — `true` plus the matching license ID, or `false, nil`.

```lua
-- Only show the option to players who actually hold the permit
local hasWeaponPermit = exports['devhub_licenses']:hasLicense('weapon_license')
if hasWeaponPermit then
    -- show "Buy Firearm" option (the server still re-checks before selling)
end
```

### getLicenseCount

```lua
local count = exports['devhub_licenses']:getLicenseCount(templateName)
```

* **What it does**: Counts the licenses the player owns, regardless of status.
* **Parameters**:
  * `templateName` *(string, optional)* — count only this template. Omit it to count every owned license.
* **Returns**: `number`.

### getMyFavorites

```lua
local favorites = exports['devhub_licenses']:getMyFavorites()
```

* **What it does**: Returns the licenses the player marked as favorites — the ones shown by `openFavoriteHand`.
* **Returns**: `table` of license data.

### showLicenseTerminal

```lua
local result = exports['devhub_licenses']:showLicenseTerminal(function(license)
    -- license.licenseId, license.template, license.originalOwner
    return { success = true, message = "Access Granted" }
end)
```

* **What it does**: Opens the license scan terminal, lets the player pick one of their licenses to scan, and hands the scanned license to your callback so you can decide whether it grants access. Use it for door readers, club entrances, bank checks or age verification.
* **Blocking**: This export **waits** for the scan or for the terminal to close, so call it from inside a thread or an event handler (never from a tight loop).
* **Parameters**:
  * `callback` *(function, optional)* — receives a table with `licenseId`, `template` and `originalOwner`. Return `{ success = boolean, message = string }` to control what the terminal displays. Without a callback the terminal shows a default success message.
* **Returns**: `table|nil`
  * On a scan: `{ license = { licenseId, template, originalOwner }, success = boolean, message = string }`.
  * When the terminal is closed instead: `{ success = false, message = "Terminal closed" }`.
* **Fires**: `OnTerminalOpened`, then `OnLicenseScanned(licenseId, templateName, licenseData)`, then `OnTerminalClosed`.

{% hint style="info" %}
A complete, ready-to-copy terminal example ships with the script in `configs/c.terminal_demo.lua` (active only while `Config.Dev` is `true`).
{% endhint %}

### closeLicenseTerminal

```lua
exports['devhub_licenses']:closeLicenseTerminal()
```

* **What it does**: Closes an open scan terminal and releases NUI focus. Any pending `showLicenseTerminal` call resolves with `{ success = false, message = "Terminal closed" }`.
* **Returns**: nothing.
* **Fires**: `OnTerminalClosed`.

***

## <mark style="color:yellow;">**Server Exports**</mark>

Call these from any server script as `exports['devhub_licenses']:<name>(...)`. These are authoritative — always use them, not the client exports, before granting anything of value.

{% hint style="warning" %}
Exports that take an `owner` **identifier** (`updateLicenseStatus`, `destroyLicense`) and the permission exports only work while that player is **loaded (online)**. They return `false` for an offline player.
{% endhint %}

### Reading player data

#### getPlayerLicenses

```lua
local licenses = exports['devhub_licenses']:getPlayerLicenses(source)
```

* **What it does**: Returns every license the player is **carrying**, including cards belonging to other people.
* **Parameters**: `source` *(number, required)* — player server ID.
* **Returns**: `table` — map of `licenseId` → license data. Empty table if the player is not loaded.

#### getPlayerOwnedLicenses

```lua
local licenses = exports['devhub_licenses']:getPlayerOwnedLicenses(source)
```

* **What it does**: Returns only the licenses the player is the original owner of.
* **Returns**: `table` — map of `licenseId` → license data.

#### getPlayerLicenseById

```lua
local license = exports['devhub_licenses']:getPlayerLicenseById(source, licenseId)
```

* **What it does**: Fetches one carried license by ID.
* **Parameters**: `source` *(number)*, `licenseId` *(string)* — both required.
* **Returns**: `table|nil`.

#### playerHasLicense

```lua
local hasIt, licenseId = exports['devhub_licenses']:playerHasLicense(source, templateName)
```

* **What it does**: The authoritative "does this player hold a valid licence" check. Returns `true` only when the player **owns** a license of that template **and** its status is `active` — suspended, revoked, lost and expired licenses fail, and so does a card taken from another player.
* **When to use**: Before selling a firearm, before letting someone fly a helicopter, before a job accepts them — any server-side gate.
* **Parameters**:
  * `source` *(number, required)* — player server ID.
  * `templateName` *(string, required)* — e.g. `"driving_license"`.
* **Returns**: `boolean, string|nil` — `true` plus the matching license ID, or `false, nil`.

```lua
-- Refuse the sale unless the buyer holds a valid weapon license
RegisterNetEvent('myshop:buyWeapon', function()
    local src = source
    local allowed = exports['devhub_licenses']:playerHasLicense(src, 'weapon_license')
    if not allowed then
        return TriggerClientEvent('esx:showNotification', src, 'You need a valid weapon license.')
    end
    -- proceed with the sale
end)
```

#### playerHasGrant

```lua
local hasGrant, status = exports['devhub_licenses']:playerHasGrant(source, templateName)
```

* **What it does**: Checks whether the player was **granted permission** for a template — the right to issue or manage that license type (e.g. a DMV employee allowed to hand out driving licenses). This is separate from owning the license itself.
* **Returns**: `boolean, string|nil` — `true` plus the grant status, or `false, nil`.

#### getPlayerGrants

```lua
local grants = exports['devhub_licenses']:getPlayerGrants(source)
```

* **What it does**: Returns all permissions granted to the player.
* **Returns**: `table` — map of `templateName` → grant data.

#### getPlayerIdentifier

```lua
local identifier = exports['devhub_licenses']:getPlayerIdentifier(source)
```

* **What it does**: Returns the identifier the license system stores the player's licenses under. Use it whenever an export asks for an `owner`.
* **Returns**: `string|nil`.

#### getPlayerLicenseCount

```lua
local count = exports['devhub_licenses']:getPlayerLicenseCount(source, templateName, status)
```

* **What it does**: Counts the licenses a player carries, with optional filters.
* **Parameters**:
  * `source` *(number, required)*.
  * `templateName` *(string, optional)* — count only this template.
  * `status` *(string, optional)* — count only licenses with this status (`active`, `suspended`, `revoked`, `lost`, `expired`).
* **Returns**: `number`.

```lua
-- How many suspended driving licenses does this player carry?
local suspended = exports['devhub_licenses']:getPlayerLicenseCount(src, 'driving_license', 'suspended')
```

### Modifying player data

#### addLicense

```lua
local ok = exports['devhub_licenses']:addLicense(source, templateName)
```

* **What it does**: Issues a new legitimate license of the given template to the player and saves it. Use it when your own script is the issuing authority — a driving school that just passed a student, an admin command, a job promotion.
* **Parameters**: `source` *(number)*, `templateName` *(string)* — both required.
* **Returns**: `boolean` — `false` if the player is not loaded or the template does not exist.
* **Fires**: `OnLicenseCreated(source, licenseId, templateName, templateLabel, licenseData, expirationDate)`.

#### updateLicenseStatus

```lua
local ok = exports['devhub_licenses']:updateLicenseStatus(source, owner, licenseId, newStatus, reason, suspensionTime)
```

* **What it does**: Changes a license's status — suspend it after a DUI, revoke it, mark it lost, or restore it to active.
* **Parameters**:
  * `source` *(number, optional)* — who performed the change (used for logs); pass `0` for system actions.
  * `owner` *(string, required)* — the **identifier** of the license owner (see `getPlayerIdentifier`). The owner must be online.
  * `licenseId` *(string, required)*.
  * `newStatus` *(string, required)* — one of `active`, `suspended`, `revoked`, `lost`, `expired`. Any other value is rejected.
  * `reason` *(string, optional)* — shown on the license and in logs.
  * `suspensionTime` *(number, optional)* — Unix timestamp for when the suspension ends.
* **Returns**: `boolean` — `false` if the owner is not loaded, the license does not exist, or the status is invalid.
* **Fires**: `OnLicenseStatusChanged(...)`.

```lua
-- Suspend a driving license for 7 days after a DUI
local identifier = exports['devhub_licenses']:getPlayerIdentifier(offenderSrc)
local hasIt, licenseId = exports['devhub_licenses']:playerHasLicense(offenderSrc, 'driving_license')
if hasIt then
    exports['devhub_licenses']:updateLicenseStatus(
        officerSrc, identifier, licenseId, 'suspended', 'Driving under the influence', os.time() + (7 * 24 * 3600)
    )
end
```

#### grantPermission

```lua
local ok = exports['devhub_licenses']:grantPermission(source, targetSource, templateName)
```

* **What it does**: Grants a player permission for a license template — for example letting a newly hired DMV clerk issue driving licenses.
* **Parameters**:
  * `source` *(number, optional)* — who is granting (for logs).
  * `targetSource` *(number, required)* — the player receiving the permission; must be online.
  * `templateName` *(string, required)*.
* **Returns**: `boolean`.
* **Fires**: `OnPermissionGranted(source, identifier, templateName, templateLabel)` — returning `false` from that hook blocks the grant.

#### revokePermission

```lua
local ok = exports['devhub_licenses']:revokePermission(source, targetSource, templateName)
```

* **What it does**: Removes a previously granted permission — for example when the clerk is fired.
* **Returns**: `boolean`.
* **Fires**: `OnPermissionRevoked(source, identifier, templateName, templateLabel)`.

#### destroyLicense

```lua
local ok = exports['devhub_licenses']:destroyLicense(source, owner, licenseId)
```

* **What it does**: Permanently destroys a license. It is gone for good — this is not the same as marking it lost or revoked, and it cannot be undone. The owner's client is re-synced immediately if they are online.
* **Parameters**:
  * `source` *(number, optional)* — who destroyed it (for logs).
  * `owner` *(string, required)* — the owner's **identifier**; the owner must be loaded.
  * `licenseId` *(string, required)*.
* **Returns**: `boolean` — `false` if the owner is not loaded or the license does not exist.
* **Fires**: `OnLicenseDestroyed(source, licenseId, templateName, identifier)`.

#### refreshPlayerLicenses

```lua
local ok = exports['devhub_licenses']:refreshPlayerLicenses(source)
```

* **What it does**: Force-pushes the player's current licenses and grants to their client. Call it after your script has changed license data through another path and the player's UI needs to catch up.
* **Returns**: `boolean`.

### Templates & metadata

#### getAllTemplates

```lua
local templates = exports['devhub_licenses']:getAllTemplates()
```

* **What it does**: Returns every registered license template.
* **Returns**: `table` — array of template data.

#### getTemplate

```lua
local template = exports['devhub_licenses']:getTemplate(templateName)
```

* **Returns**: `table|nil` — the template's data, or `nil` if it does not exist.

#### templateExists

```lua
local exists = exports['devhub_licenses']:templateExists(templateName)
```

* **What it does**: Cheap existence check — useful for validating admin command input before calling `addLicense`.
* **Returns**: `boolean`.

#### getDefaultLicenses

```lua
local names = exports['devhub_licenses']:getDefaultLicenses()
```

* **What it does**: Returns the template names configured to be issued automatically to new players.
* **Returns**: `table` — array of template names.

#### getFakeableLicenses

```lua
local names = exports['devhub_licenses']:getFakeableLicenses()
```

* **What it does**: Returns the template names that can be forged as fake IDs.
* **Returns**: `table` — array of template names.

#### getDataFields

```lua
local fields = exports['devhub_licenses']:getDataFields()
```

* **What it does**: Returns the configured `Config.DataFields` structure — the fields printed on licenses (name, date of birth, blood type…).
* **Returns**: `table` — array of `{ name, label, default, showInProfile, liveProfileDisplay }`.

#### getPlayerLiveData

```lua
local data = exports['devhub_licenses']:getPlayerLiveData(source)
```

* **What it does**: Runs each data field's `getData()` function for the player and returns the current values. Use it to preview what a freshly issued license would print. If a field's function errors, its configured `default` is returned instead.
* **Returns**: `table` — map of field name → value.

### Session helpers

#### isPlayerLoaded

```lua
local loaded = exports['devhub_licenses']:isPlayerLoaded(source)
```

* **What it does**: Reports whether the player's license data has finished loading. Check this before acting on a player who just connected.
* **Returns**: `boolean`.

#### getSourceByIdentifier

```lua
local src = exports['devhub_licenses']:getSourceByIdentifier(identifier)
```

* **What it does**: Resolves an identifier back to a server ID.
* **Returns**: `number|nil` — `nil` if that player is offline.

#### getLoadedPlayers

```lua
local identifiers = exports['devhub_licenses']:getLoadedPlayers()
```

* **Returns**: `table` — array of identifiers of every player currently loaded in the license system.

***

## <mark style="color:yellow;">**Client Open Functions**</mark>

Edit the `OpenClientFunctions` table in `configs/c.functions.lua`. Each entry is called at a specific
moment on the player's own machine. Hooks marked **blocking** decide whether the action proceeds:
return `false` to stop it, `true` (or nothing) to allow it.

| Hook                                                            | Fires when                                                     | Blocking |
| --------------------------------------------------------------- | -------------------------------------------------------------- | -------- |
| `OnMdtOpened(isLaptopApp)`                                       | The MDT tablet opens.                                           | –        |
| `OnMdtClosed()`                                                  | The MDT tablet closes.                                          | –        |
| `OnCardHolderOpened(licenseCount)`                               | The player's own card holder opens.                             | –        |
| `OnCardHolderClosed()`                                           | The card holder closes.                                         | –        |
| `OnFavoriteHandOpened(favoriteCount)`                            | The favorite-hand display starts.                               | –        |
| `OnFavoriteHandClosed()`                                         | The favorite-hand display ends.                                 | –        |
| `OnShowLicense(licenseId)`                                       | The player starts showing a license to others.                  | ✅        |
| `OnLicenseDisplayReceived(templateData, licenseData, sourceId)`  | Someone else's license appears on **your** screen.              | –        |
| `OnNewLicensePreview(previewData)`                               | The "new license" preview animation plays after a pickup.       | –        |
| `OnCheckPlayerLicenses(targetServerId, targetEntity)`            | The player starts searching another player.                     | ✅        |
| `OnViewingPlayerLicenses(targetServerId, playerName, count)`     | The searched player's licenses have been fetched and shown.     | –        |
| `OnTerminalOpened()`                                             | A scan terminal opens.                                          | –        |
| `OnLicenseScanned(licenseId, templateName, licenseData)`         | A license is scanned at a terminal.                             | –        |
| `OnTerminalClosed()`                                             | The terminal closes.                                            | –        |
| `OnScreenshotStarted()`                                          | Before the avatar photo — the player is moved to the studio.    | –        |
| `OnScreenshotCompleted(imageUrl)`                                | After the photo — the player is back at their original spot.    | –        |
| `OnHideoutDoorHandler(entity)`                                   | Repeatedly, to decide if the hideout door option is shown.      | ✅        |
| `OnHideoutCarpetHandler(entity, distance, coords)`               | Repeatedly, to decide if the hideout carpet option is shown.    | ✅        |

{% hint style="danger" %}
`OnHideoutDoorHandler` and `OnHideoutCarpetHandler` are called **continuously** by the target system to decide whether the option is visible — not once when the player interacts. Keep them cheap: no loops, no waits, no server calls.
{% endhint %}

### Blocking an action

```lua
-- configs/c.functions.lua

---Called WHEN the player starts showing a license to nearby players.
OnShowLicense = function(licenseId)
    -- Don't let players flash their ID while driving
    if IsPedInAnyVehicle(PlayerPedId(), false) then
        return false
    end
    return true
end,

---Called WHEN the player starts searching another player.
OnCheckPlayerLicenses = function(targetServerId, targetEntity)
    -- Only allow searching a player who has their hands up
    if not IsEntityPlayingAnim(targetEntity, 'random@mugging3', 'handsup_standing_base', 3) then
        return false
    end
    return true
end,
```

### Reacting to an action

```lua
-- configs/c.functions.lua

---Called AFTER the avatar photo has been taken and uploaded.
OnScreenshotCompleted = function(imageUrl)
    if not imageUrl then
        return Core.Notify('Photo upload failed — try again.', 3000, 'error')
    end
    PlaySoundFrontend(-1, 'CHECKPOINT_PERFECT', 'HUD_MINI_GAME_SOUNDSET', true)
end,

---Called WHEN a license is scanned at a terminal.
OnLicenseScanned = function(licenseId, templateName, licenseData)
    if templateName == 'vip_pass' then
        PlaySoundFrontend(-1, 'RANK_UP', 'HUD_AWARDS', true)
    end
end,
```

***

## <mark style="color:yellow;">**Server Open Functions**</mark>

Edit the `OpenServerFunctions` table in `configs/s.functions.lua`. These run on the server, so they
are the right place for anything that must not be trusted to the client. By default each one sends a
Discord log through `Config.LogsWebhook`. Hooks marked **blocking** can stop the action by returning
`false`.

| Hook                                                                                              | Fires when                                                           | Blocking |
| -------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- | -------- |
| `OnPlayerLoaded(source, identifier, licenses, granted)`                                            | A player's license data finished loading from the database.          | –        |
| `OnPlayerUnloaded(source, identifier)`                                                             | Just **before** a player's data is saved and unloaded.               | –        |
| `CanPickupLicense(source, templateName, paymentMethod, chargeAmount)`                              | **Before** a player picks up a license at the city hall NPC.         | ✅        |
| `OnLicensePickedUp(source, templateName, paymentMethod, chargeAmount)`                             | After the pickup succeeded and payment was taken.                    | –        |
| `OnLicenseCreated(source, licenseId, templateName, templateLabel, licenseData, expirationDate)`    | After a legitimate license was created and saved.                    | –        |
| `OnFakeLicenseCreated(source, licenseId, templateName, templateLabel, fakeData)`                   | After a **fake** license was forged and saved.                       | –        |
| `OnFakeIdMissionCompleted(source, licenseId, templateName, payment)`                               | After a fake-ID mission was delivered and paid out.                  | –        |
| `OnLicenseStatusChanged(source, owner, licenseId, templateName, oldStatus, newStatus, reason, suspensionTime)` | After a status change (suspended, revoked, expired…).    | –        |
| `OnLicenseDestroyed(source, licenseId, templateName, identifier)`                                  | After a license was permanently destroyed.                           | –        |
| `OnLicenseGiven(source, targetSource, licenseId, templateName, giverIdentifier, receiverIdentifier)` | After a license was handed to another player.                       | –        |
| `OnLicenseTaken(source, targetSource, licenseId, templateName, takerIdentifier, victimIdentifier)` | After a license was taken/confiscated from another player.           | –        |
| `OnLicenseShown(source, licenseId, templateName, targetServerId)`                                  | When a player shows a license (`targetServerId` is `nil` = to all nearby). | ✅  |
| `OnPlayerLicensesChecked(source, targetServerId)`                                                  | When a player tries to check someone else's licenses.                | ✅        |
| `OnPermissionGranted(source, identifier, templateName, templateLabel)`                             | When a template permission is granted.                               | ✅        |
| `OnPermissionRevoked(source, identifier, templateName, templateLabel)`                             | When a template permission is revoked.                               | –        |
| `OnTemplateSaved(source, templateName, templateData, isUpdate)`                                    | After an admin created or updated a template (`isUpdate` tells which). | –      |

{% hint style="info" %}
In `OnLicenseStatusChanged`, `source` may be `nil` when the change was automatic — an expiry running out or a suspension ending on its own, with nobody triggering it.
{% endhint %}

### Gate who may pick up a license

```lua
-- configs/s.functions.lua

---Called BEFORE a player picks up (creates) a license at the city hall NPC.
CanPickupLicense = function(source, templateName, paymentMethod, chargeAmount)
    -- Require passing the driving school before a driving license can be issued
    if templateName == 'driving_license' and not exports['driving_school']:hasPassed(source) then
        TriggerClientEvent('esx:showNotification', source, 'You must pass the driving test first.')
        return false
    end
    return true
end,
```

### Restrict license checks to police

```lua
-- configs/s.functions.lua

---Called WHEN a player tries to check another player's licenses.
OnPlayerLicensesChecked = function(source, targetServerId)
    local job = Core.GetJob(source)
    if job.name ~= 'police' then
        TriggerClientEvent('esx:showNotification', source, 'Only law enforcement may check licenses.')
        return false
    end
    return true
end,
```

### Sync issued licenses with your own systems

```lua
-- configs/s.functions.lua

---Called AFTER a legitimate license has been created and saved.
OnLicenseCreated = function(source, licenseId, templateName, templateLabel, licenseData, expirationDate)
    -- Mirror the license into your MDT
    exports['my_mdt']:addRecord(source, {
        type = 'license',
        id = licenseId,
        label = templateLabel,
        expires = expirationDate > 0 and expirationDate or nil,
    })
end,

---Called AFTER a license was confiscated from another player.
OnLicenseTaken = function(source, targetSource, licenseId, templateName, takerIdentifier, victimIdentifier)
    -- Log the confiscation to your police report system
    exports['my_reports']:log(source, ('Confiscated %s (%s) from %s'):format(templateName, licenseId, victimIdentifier))
end,
```

***

## <mark style="color:yellow;">**Integration Recipes**</mark>

### Weapon shop that requires a valid firearm license

```lua
-- server-side, in your shop resource
RegisterNetEvent('myshop:buyWeapon', function(weapon)
    local src = source

    if not exports['devhub_licenses']:isPlayerLoaded(src) then
        return TriggerClientEvent('esx:showNotification', src, 'Your licenses are still loading.')
    end

    local hasLicense = exports['devhub_licenses']:playerHasLicense(src, 'weapon_license')
    if not hasLicense then
        return TriggerClientEvent('esx:showNotification', src, 'A valid firearm license is required.')
    end

    -- sell the weapon
end)
```

### Police: suspend a driving license after a DUI

```lua
-- server-side
RegisterNetEvent('mypolice:issueDui', function(offenderId)
    local officer = source

    local identifier = exports['devhub_licenses']:getPlayerIdentifier(offenderId)
    local hasIt, licenseId = exports['devhub_licenses']:playerHasLicense(offenderId, 'driving_license')
    if not identifier or not hasIt then
        return TriggerClientEvent('esx:showNotification', officer, 'This person holds no valid driving license.')
    end

    exports['devhub_licenses']:updateLicenseStatus(
        officer, identifier, licenseId, 'suspended',
        'Driving under the influence',
        os.time() + (7 * 24 * 60 * 60) -- 7 days
    )
end)
```

### Door reader that only opens for club members

```lua
-- client-side, on your door target
RegisterNetEvent('myclub:scanAtDoor', function()
    local result = exports['devhub_licenses']:showLicenseTerminal(function(license)
        if license.template ~= 'club_membership' then
            return { success = false, message = 'Members only' }
        end
        return { success = true, message = 'Welcome back' }
    end)

    if result and result.success then
        TriggerServerEvent('myclub:openDoor')
    end
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
