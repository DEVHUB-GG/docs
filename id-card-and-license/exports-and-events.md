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

## <mark style="color:yellow;">**Result contract**</mark>

Every client and server export returns its documented value(s) **first**, followed by two trailing values so your integration can always tell success from failure and _why_ something failed:

```lua
local value, success, error = exports['devhub_licenses']:someExport(...)
```

* `success` *(boolean)* — `true` when the export did what you asked, `false` when it could not.
* `error` *(string | nil)* — a stable, machine-readable code when `success` is `false`, otherwise `nil`.

This is **fully backwards-compatible**: existing code that captures only the first value (`local licenses = exports[...]:getPlayerLicenses(src)`) keeps working — the extra values are simply ignored. Capture the two trailing values only when you want the reliability signal.

Because the primary value comes first, `success` distinguishes a genuinely **empty** result from a **failed** call — an empty `{}` with `success = true` means "the player has no licenses", while `{}` with `success = false, error = "player_not_loaded"` means "the player was not loaded". A `false` from a check like `playerHasLicense` with `success = true` means "does not hold it", while `success = false, error = "player_not_loaded"` means "could not check yet".

{% hint style="info" %}
The error codes are part of the export contract and are intentionally **not translated** — match on them in code (`if err == 'template_not_found' then`), and use them to pick which localized message to show your own players.
{% endhint %}

| `error` code         | Meaning                                                         |
| -------------------- | -------------------------------------------------------------- |
| `invalid_source`     | The player server ID was missing or invalid.                   |
| `missing_arguments`  | A required argument (template name, license ID, …) was missing.|
| `player_not_loaded`  | The target player is not loaded / offline.                     |
| `template_not_found` | No license template with that name exists.                     |
| `license_not_found`  | No license with that ID exists, or the player is not carrying it.|
| `not_owner`          | The license does not belong to the given owner.                |
| `invalid_status`     | The status is not one of `active`, `suspended`, `revoked`, `lost`, `expired`. |
| `creation_failed`    | The license record could not be created (database error).      |

{% hint style="warning" %}
`showLicenseTerminal` and `closeLicenseTerminal` are the one exception: the blocking terminal pair keeps its own documented result table (`{ success, message }`) instead of the trailing `success, error` values. See their entries below.
{% endhint %}

### Automatic failure notifications

When an export fails, the affected player is also shown a `Core.Notify` explaining why — controlled by [`Config.NotifyOnExportFailure`](configuration/sh.main.lua.md) (`true` by default). On the client this is the local caller; on the server it is the `source` player passed to the export (system calls and the template/metadata exports never notify, and the side-effect-free `isPlayerLoaded` never notifies). The `success, error` return values are unaffected — turn the config off if your integration handles messaging itself or calls these exports in a loop. The notification text lives in the `export_fail_*` keys of `sh.lang.lua`.

***

## <mark style="color:yellow;">**Which check do I use?**</mark>

A player's relationship with a license has **three independent layers**, and each layer has its own check. Picking the wrong one is the most common integration mistake:

| You want to know                                                                  | Layer                  | Check with                                                     |
| --------------------------------------------------------------------------------- | ---------------------- | -------------------------------------------------------------- |
| "May this player hold this license type at all?" (passed the exam / was approved) | **Permission (grant)** | `playerHasGrant` / `getPlayerGrants` *(server)*                |
| "Does this player hold a **valid** license right now?"                            | **License status**     | `playerHasLicense` *(server)* — `hasLicense` *(client, UI only)* |
| "Is the physical card in this player's pockets?"                                  | **Physical item**      | Your inventory system — see below                              |

### Permission checks — `playerHasGrant`

A **grant** is the player's approval to hold a license type. It is created when they pass the exam, when a default license is auto-issued on their first join, or when an official (or your script, via `grantPermission`) approves them. The grant is what makes the License Pickup clerk offer that template — including re-taking a replacement card after the license was lost. It is **not** the license itself: a player can be granted `driving_license` while their actual card is suspended, lost, or not yet picked up.

Use `playerHasGrant` (one template) or `getPlayerGrants` (all templates) when you care about **entitlement**, not the card — e.g. skipping your driving-school flow for someone already approved, or deciding whether a replacement may be issued.

### License status checks — `playerHasLicense`

The **license record** is the card itself, and it carries a status (`active`, `suspended`, `revoked`, `lost`, `expired`). `playerHasLicense` is the authoritative gameplay gate: `true` only when the player **owns** a license of that template **and** it is currently `active`. A suspended, revoked, lost or expired card fails, someone else's card the player happens to carry fails, and a forged fake ID always fails (a fake is never owned by its holder). Use it before anything of value: selling a weapon, deciding a traffic stop, accepting someone into a job.

Use the client-side `hasLicense` **only** to shape UI (hide a menu entry, gray out a button). It reads the local player's own copy and must never be the final authority — the server re-checks with `playerHasLicense`.

### Physical item checks — your inventory

With [`Config.UsePhysicalLicenses = true`](configuration/sh.main.lua.md) each license also exists as an **inventory item** (the item per template is set in the license creator's general settings; the script stamps it with `licenseId`, `templateName`, `label` and `owner` metadata). The License System deliberately exposes **no export for item possession** — the card lives in your framework's inventory, so check it there (e.g. `xPlayer.getInventoryItem(...)` or `exports.ox_inventory:Search(...)`).

{% hint style="warning" %}
Holding the plastic proves nothing. The card in the pocket may be someone else's, forged, or backed by a suspended or revoked record — and a perfectly valid license may currently sit in another player's pocket. Item checks are for roleplay flows (confiscating the card, handing it over); **validity is always `playerHasLicense`**.
{% endhint %}

The three checks side by side:

```lua
-- server-side
local src = source

-- 1) Entitlement: may they hold it at all? (exam passed / approved)
local approved = exports['devhub_licenses']:playerHasGrant(src, 'weapon_license')

-- 2) Validity: do they own an ACTIVE card right now?  ← the gameplay gate
local valid = exports['devhub_licenses']:playerHasLicense(src, 'weapon_license')

-- 3) Possession: is the physical card in their pockets? (physical mode only — ask your inventory)
local carried = exports.ox_inventory:Search(src, 'count', 'weapon_license_item') > 0
```

The layers move together through the built-in flows: `issueLicense` sets all three (grant + active record + physical item), `revokeLicense` clears all three, while `updateLicenseStatus` touches only the record and `grantPermission` / `revokePermission` touch only the grant.

***

## <mark style="color:yellow;">**Client Exports**</mark>

Call these from any client script as `exports['devhub_licenses']:<name>(...)`.

Each row's **Returns** column lists the documented primary value(s); unless noted, every export also appends the trailing `success, error` values described in [Result contract](#result-contract).

| Export                              | Returns                | Purpose                                     |
| ----------------------------------- | ---------------------- | ------------------------------------------- |
| `openMdt(isLaptopApp)`              | `boolean`              | Open the license MDT tablet.                |
| `openCardHolder()`                  | `boolean`              | Open the player's own card holder.          |
| `openCardHolderForPlayer(serverId)` | `boolean`              | Open another player's card holder.          |
| `openFavoriteHand()`                | `boolean`              | Show the favorite licenses in hand.         |
| `getMyLicenses()`                   | `table`                | Licenses the player **owns**.               |
| `getAllMyLicenses()`                | `table`                | All licenses the player **carries**.        |
| `getLicenseById(licenseId)`         | `table\|nil`           | One carried license.                        |
| `hasLicense(templateName)`          | `boolean, string\|nil` | Active license of a template?               |
| `getLicenseCount(templateName)`     | `number`               | How many owned licenses.                    |
| `getMyFavorites()`                  | `table`                | The player's favorited licenses.            |
| `showLicenseTerminal(callback)`     | `table\|nil` *(no trailing values)* | Open a scan terminal and wait for the scan. |
| `closeLicenseTerminal()`            | – *(no trailing values)*            | Close the scan terminal.                    |

### openMdt

```lua
exports['devhub_licenses']:openMdt(isLaptopApp)
```

* **What it does**: Opens the main license MDT tablet — the interface used to browse, create and manage licenses.
* **Parameters**:
  * `isLaptopApp` *(boolean, optional)* — pass `true` when opening the MDT from inside `devhub_laptop` so it renders as an app rather than a standalone tablet. Defaults to `false`.
* **Returns**: `boolean, string|nil` — `true, nil` once the MDT open is triggered.
* **Fires**: `OnMdtOpened(isLaptopApp)`.

### openCardHolder

```lua
exports['devhub_licenses']:openCardHolder()
```

* **What it does**: Opens the card holder showing the local player's licenses — the same UI used when the player inspects their own wallet.
* **Returns**: `boolean, string|nil` — `true, nil` once the card holder open is triggered.
* **Fires**: `OnCardHolderOpened(licenseCount)`.

### openCardHolderForPlayer

```lua
exports['devhub_licenses']:openCardHolderForPlayer(targetServerId)
```

* **What it does**: Opens the card holder showing **another player's** licenses. Use it for police searches or any inspection flow driven by your own script instead of the built-in target option.
* **Parameters**:
  * `targetServerId` *(number, required)* — server ID of the player being inspected.
* **Returns**: `boolean, string|nil` — `true, nil` once the search is triggered, or `false, "missing_arguments"` if `targetServerId` was omitted.
* **Fires**: `OnCheckPlayerLicenses` and, on the server, `OnPlayerLicensesChecked` — either can block the request.

### openFavoriteHand

```lua
exports['devhub_licenses']:openFavoriteHand()
```

* **What it does**: Plays the "hold licenses in hand" display with the player's favorited licenses, so nearby players can read them.
* **Returns**: `boolean, string|nil` — `true, nil` once the favorite-hand display is triggered.
* **Fires**: `OnFavoriteHandOpened(favoriteCount)`.

### getMyLicenses

```lua
local licenses = exports['devhub_licenses']:getMyLicenses()
```

* **What it does**: Returns only the licenses the player **originally owns** — licenses issued to them, not ones handed over or confiscated from someone else.
* **Returns**: `table, boolean, string|nil` — map of `licenseId` → license data (empty `{}` with `success = false, error = "player_not_loaded"` if the player is not loaded yet).

### getAllMyLicenses

```lua
local licenses = exports['devhub_licenses']:getAllMyLicenses()
```

* **What it does**: Returns **everything the player is currently carrying**, including licenses that belong to other people (given, found or confiscated).
* **Returns**: `table, boolean, string|nil` — map of `licenseId` → license data (empty `{}` with `error = "player_not_loaded"` if the player is not loaded yet).

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
* **Returns**: `table|nil, boolean, string|nil` — the license data with `success = true`, or `nil` with `error = "missing_arguments"` (no ID), `"player_not_loaded"`, or `"license_not_found"` (the player is not carrying it).

### hasLicense

```lua
local hasIt, licenseId = exports['devhub_licenses']:hasLicense(templateName)
```

* **What it does**: Checks whether the player holds an **active** license of the given template that they **own**. A suspended, revoked, lost or expired license returns `false`, and so does someone else's card the player happens to be carrying.
* **When to use**: Gating menus, prompts and target options client-side — for example hiding "Drive a taxi" from a player without a taxi permit.
* **Parameters**:
  * `templateName` *(string, required)* — the template's unique name, e.g. `"driving_license"`.
* **Returns**: `boolean, string|nil, boolean, string|nil` — `hasLicense, licenseId, success, error`. `true, licenseId, true, nil` when held; `false, nil, true, nil` when successfully checked but not held; `false, nil, false, error` when the check could not run (`"missing_arguments"`, `"player_not_loaded"`).

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
* **Returns**: `number, boolean, string|nil` — the count with `success = true`, or `0` with `error = "player_not_loaded"`.

### getMyFavorites

```lua
local favorites = exports['devhub_licenses']:getMyFavorites()
```

* **What it does**: Returns the licenses the player marked as favorites — the ones shown by `openFavoriteHand`.
* **Returns**: `table, boolean, string|nil` — favorites map with `success = true`, or empty `{}` with `error = "player_not_loaded"`.

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
* **Returns**: `table|nil` — this export keeps its own result table instead of the trailing `success, error` values:
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
Every export that targets a player (`addLicense`, `issueLicense`, `updateLicenseStatus`, `revokeLicense`, `grantPermission`, `revokePermission`, and the readers) only works while that player is **loaded (online)** — it fails with `"player_not_loaded"` otherwise. (`destroyLicense` and `HasLicense` are the exceptions: they work against the database directly, whether the owner is online or not.)
{% endhint %}

Every server export also appends the trailing `success, error` values described in [Result contract](#result-contract).

### Reading player data

#### getPlayerLicenses

```lua
local licenses = exports['devhub_licenses']:getPlayerLicenses(source)
```

* **What it does**: Returns every license the player is **carrying**, including cards belonging to other people.
* **Parameters**: `source` *(number, required)* — player server ID.
* **Returns**: `table, boolean, string|nil` — map of `licenseId` → license data. Empty `{}` with `error = "invalid_source"` or `"player_not_loaded"` on failure.

#### getPlayerOwnedLicenses

```lua
local licenses = exports['devhub_licenses']:getPlayerOwnedLicenses(source)
```

* **What it does**: Returns only the licenses the player is the original owner of.
* **Returns**: `table, boolean, string|nil` — map of `licenseId` → license data (empty `{}` with `error = "invalid_source"` / `"player_not_loaded"` on failure).

#### getPlayerLicenseById

```lua
local license = exports['devhub_licenses']:getPlayerLicenseById(source, licenseId)
```

* **What it does**: Fetches one carried license by ID.
* **Parameters**: `source` *(number)*, `licenseId` *(string)* — both required.
* **Returns**: `table|nil, boolean, string|nil` — the license with `success = true`, or `nil` with `error` one of `"invalid_source"`, `"missing_arguments"`, `"player_not_loaded"`, `"license_not_found"` (the player is not carrying it).

#### playerHasLicense

```lua
local hasIt, licenseId = exports['devhub_licenses']:playerHasLicense(source, templateName)
```

* **What it does**: The authoritative "does this player hold a valid licence" check. Returns `true` only when the player **owns** a license of that template **and** its status is `active` — suspended, revoked, lost and expired licenses fail, a card taken from another player fails, and a forged fake ID always fails.
* **When to use**: Before selling a firearm, before letting someone fly a helicopter, before a job accepts them — any server-side gate. See [Which check do I use?](#which-check-do-i-use) for how this differs from a grant or an item check.
* **Parameters**:
  * `source` *(number, required)* — player server ID.
  * `templateName` *(string, required)* — e.g. `"driving_license"`.
* **Returns**: `boolean, string|nil, boolean, string|nil` — `hasLicense, licenseId, success, error`. `true, licenseId, true, nil` when held; `false, nil, true, nil` when checked but not held; `false, nil, false, error` when the check could not run (`"invalid_source"`, `"missing_arguments"`, `"player_not_loaded"`). Always check the third value before trusting a `false` first value on a freshly-connected player.

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

#### HasLicense

```lua
local hasIt = exports['devhub_licenses']:HasLicense(identifier, licenseName)
```

* **What it does**: The **identifier-keyed** twin of `playerHasLicense` — checks whether the player
  behind an identifier holds an **active**, owned, non-fake license. Because it looks the license up
  by identifier rather than server ID, it works **even while that player is offline**.
* **When to use**: Gates that run without a live player — catalog filters, marketplace listings,
  offline audits — or integrations configured with an export name (devhub\_shops' `Config.Licenses`
  gate calls exactly this export). For an online player you already have a `source` for, prefer
  `playerHasLicense`.
* **Parameters**:
  * `identifier` *(string, required)* — the owner's identifier (see `getPlayerIdentifier`).
  * `licenseName` *(string, required)* — a template name (e.g. `"driving_license"`), or an
    esx\_license type mapped in [`Config.Compat.EsxLicense.typeMap`](configuration/s.compat.lua.md)
    (e.g. `"drive"`).
* **Returns**: `boolean, boolean, string|nil` — `hasLicense, success, error`. `false, false,
  "missing_arguments"` when a parameter is missing, `false, false, "not_ready"` if the compatibility
  layer has not finished starting (only possible in the first seconds after a resource start).
* **Blocking**: During resource startup this export **waits** (up to 10 seconds) for the
  compatibility layer to come up — call it from a thread or event handler, not a tight loop.

#### playerHasGrant

```lua
local hasGrant, status = exports['devhub_licenses']:playerHasGrant(source, templateName)
```

* **What it does**: Checks whether the player has been **approved to hold** a license type — the grant created by passing the exam, by a default license on first join, or by an approval (in the MDT or via `grantPermission`). A grant is not the card: it survives the card being suspended, lost or destroyed, and it is what makes the pickup clerk offer (re-)taking that template. See [Which check do I use?](#which-check-do-i-use).
* **When to use**: Entitlement questions — "has this player passed the exam / been approved?", "may the clerk hand them a replacement?" — as opposed to "is their card valid right now" (`playerHasLicense`).
* **Returns**: `boolean, string|nil, boolean, string|nil` — `hasGrant, status, success, error`. `true, nil, true, nil` when granted (`status` is only populated for legacy grant data — rely on the first value); `false, nil, true, nil` when checked but not granted; `false, nil, false, error` when the check could not run (`"invalid_source"`, `"missing_arguments"`, `"player_not_loaded"`).

#### getPlayerGrants

```lua
local grants = exports['devhub_licenses']:getPlayerGrants(source)
```

* **What it does**: Returns every license template the player has been approved to hold (see [Which check do I use?](#which-check-do-i-use)).
* **Returns**: `table, boolean, string|nil` — map of `templateName` → grant data (empty `{}` with `error = "invalid_source"` / `"player_not_loaded"` on failure).

#### getPlayerIdentifier

```lua
local identifier = exports['devhub_licenses']:getPlayerIdentifier(source)
```

* **What it does**: Returns the identifier the license system stores the player's licenses under. Use it whenever an export asks for an `owner`.
* **Returns**: `string|nil, boolean, string|nil` — the identifier with `success = true`, or `nil` with `error = "invalid_source"` / `"player_not_loaded"`.

#### getPlayerLicenseCount

```lua
local count = exports['devhub_licenses']:getPlayerLicenseCount(source, templateName, status)
```

* **What it does**: Counts the licenses a player carries, with optional filters.
* **Parameters**:
  * `source` *(number, required)*.
  * `templateName` *(string, optional)* — count only this template.
  * `status` *(string, optional)* — count only licenses with this status (`active`, `suspended`, `revoked`, `lost`, `expired`).
* **Returns**: `number, boolean, string|nil` — the count with `success = true`, or `0` with `error = "invalid_source"` / `"player_not_loaded"`.

```lua
-- How many suspended driving licenses does this player carry?
local suspended = exports['devhub_licenses']:getPlayerLicenseCount(src, 'driving_license', 'suspended')
```

### Modifying player data

#### addLicense

```lua
local ok = exports['devhub_licenses']:addLicense(source, templateName, options)
```

* **What it does**: Creates a new legitimate license **record** of the given template for the player and saves it (when [`Config.UsePhysicalLicenses`](configuration/sh.main.lua.md) is on, the physical card item is handed over too). It does **not** grant the permission — for the full grant + record + photo flow use `issueLicense` below.
* **Parameters**:
  * `source` *(number, required)* — player server ID; must be online.
  * `templateName` *(string, required)*.
  * `options` *(table, optional)* — omit it for the classic no-photo behaviour:
    * `avatarUrl` *(string)* — embed a ready-made photo on the license.
    * `takePhoto` *(boolean)* — `true` sends the player to the photo studio; the export still returns immediately and the captured photo is patched onto the license afterwards.
* **Returns**: `boolean, string|nil` — `true, nil` on success, or `false` with `error` one of `"invalid_source"`, `"missing_arguments"`, `"player_not_loaded"`, `"template_not_found"`, `"creation_failed"`.
* **Fires**: `OnLicenseCreated(source, licenseId, templateName, templateLabel, licenseData, expirationDate)`.

#### issueLicense

```lua
local licenseId = exports['devhub_licenses']:issueLicense(source, targetSource, templateName, options)
```

* **What it does**: Issues a license **end-to-end in one call**: grants the permission (so the license is legitimate and re-takeable at the pickup clerk), creates the license record, hands over the physical card item (when [`Config.UsePhysicalLicenses`](configuration/sh.main.lua.md) is on), starts avatar photo mode, applies the expiration and returns the new license ID. This is the one-liner for "this player just earned a license" — a driving school, an admin command, a job promotion.
* **Parameters**:
  * `source` *(number, optional)* — acting player (history/logs); pass `0` or `nil` for system actions.
  * `targetSource` *(number, required)* — the player receiving the license; must be online.
  * `templateName` *(string, required)*.
  * `options` *(table, optional)*:
    * `avatarUrl` *(string)* — embed a ready-made photo instead of taking one.
    * `takePhoto` *(boolean)* — photo mode is **on by default**; pass `false` to skip it. The export returns immediately either way; the captured photo is patched onto the license afterwards.
    * `expirationDate` *(number)* — Unix timestamp overriding the template's expiration.
    * `expirationDays` *(number)* — expire this many days from now.
* **Returns**: `string|nil, boolean, string|nil` — the new license ID with `success = true`, or `nil` with `error` one of `"invalid_source"`, `"missing_arguments"`, `"player_not_loaded"`, `"template_not_found"`, `"creation_failed"`.
* **Fires**: `OnPermissionGranted(...)`, then `OnLicenseCreated(...)`.

```lua
-- Driving school: student passed — issue the license, valid for 90 days
local licenseId = exports['devhub_licenses']:issueLicense(0, studentSrc, 'driving_license', {
    expirationDays = 90,
})
```

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
* **Returns**: `boolean, string|nil` — `true, nil` on success, or `false` with `error` one of `"missing_arguments"`, `"player_not_loaded"` (owner offline), `"invalid_status"`.
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

* **What it does**: Approves a player to **hold** a license template — the same grant they would earn by passing the exam. From then on the pickup clerk offers them that license (and replacements). It does **not** create the license record itself — use `addLicense` for just the record, or `issueLicense` for grant + record in one call. See [Which check do I use?](#which-check-do-i-use).
* **Parameters**:
  * `source` *(number, optional)* — who is granting (for logs).
  * `targetSource` *(number, required)* — the player receiving the permission; must be online.
  * `templateName` *(string, required)*.
* **Returns**: `boolean, string|nil` — `true, nil` on success, or `false` with `error` one of `"invalid_source"` (no `targetSource`), `"missing_arguments"`, `"player_not_loaded"`.
* **Fires**: `OnPermissionGranted(source, identifier, templateName, templateLabel)` — returning `false` from that hook blocks the grant.

#### revokePermission

```lua
local ok = exports['devhub_licenses']:revokePermission(source, targetSource, templateName)
```

* **What it does**: Withdraws the player's approval for a template, which also removes it from the pickup clerk's retake offer. Existing license records and physical cards are **untouched** — use `revokeLicense` below to strip everything at once.
* **Returns**: `boolean, string|nil` — `true, nil` on success, or `false` with `error` one of `"invalid_source"` (no `targetSource`), `"missing_arguments"`, `"player_not_loaded"`.
* **Fires**: `OnPermissionRevoked(source, identifier, templateName, templateLabel)`.

#### revokeLicense

```lua
local ok = exports['devhub_licenses']:revokeLicense(source, targetSource, templateName, reason)
```

* **What it does**: Fully revokes a license template from a player in one call — the mirror of `issueLicense`. It withdraws the grant (so the pickup clerk no longer offers a replacement), marks every license record the player owns of that template as `revoked`, and removes the physical card item(s) from their inventory (when [`Config.UsePhysicalLicenses`](configuration/sh.main.lua.md) is on). Use it when the player must lose the license entirely — a court verdict, leaving a whitelist job.
* **Parameters**:
  * `source` *(number, optional)* — acting player (history/logs); pass `0` or `nil` for system actions.
  * `targetSource` *(number, required)* — the player losing the license; must be online.
  * `templateName` *(string, required)*.
  * `reason` *(string, optional)* — recorded on the status change and in history.
* **Returns**: `boolean, string|nil` — `true, nil` on success, or `false` with `error` one of `"invalid_source"`, `"missing_arguments"`, `"player_not_loaded"`, `"template_not_found"`.
* **Fires**: `OnPermissionRevoked(...)`, then `OnLicenseStatusChanged(...)` for each license record it revokes.

{% hint style="info" %}
Suspending is not revoking: for a temporary punishment (DUI, points) use `updateLicenseStatus` with `suspended` — the grant stays, so the license can return to `active` later. `revokeLicense` takes the entitlement away entirely.
{% endhint %}

#### destroyLicense

```lua
local ok = exports['devhub_licenses']:destroyLicense(source, owner, licenseId)
```

* **What it does**: Permanently destroys a license. It is gone for good — this is not the same as marking it lost or revoked, and it cannot be undone. The owner's client is re-synced immediately if they are online.
* **Parameters**:
  * `source` *(number, optional)* — who destroyed it (for logs).
  * `owner` *(string, required)* — the owner's **identifier**; works even if the owner is offline (the row is detached in the DB).
  * `licenseId` *(string, required)*.
* **Returns**: `boolean, string|nil` — `true, nil` on success, or `false` with `error` one of `"missing_arguments"`, `"license_not_found"`, `"not_owner"` (the license belongs to someone else).
* **Fires**: `OnLicenseDestroyed(source, licenseId, templateName, identifier)`.

#### refreshPlayerLicenses

```lua
local ok = exports['devhub_licenses']:refreshPlayerLicenses(source)
```

* **What it does**: Force-pushes the player's current licenses and grants to their client. Call it after your script has changed license data through another path and the player's UI needs to catch up.
* **Returns**: `boolean, string|nil` — `true, nil` on success, or `false` with `error = "invalid_source"` / `"player_not_loaded"`.

### Templates & metadata

#### getAllTemplates

```lua
local templates = exports['devhub_licenses']:getAllTemplates()
```

* **What it does**: Returns every registered license template.
* **Returns**: `table, boolean, string|nil` — array of template data; always `success = true`.

#### getTemplate

```lua
local template = exports['devhub_licenses']:getTemplate(templateName)
```

* **Returns**: `table|nil, boolean, string|nil` — the template's data with `success = true`, or `nil` with `error = "missing_arguments"` / `"template_not_found"`.

#### templateExists

```lua
local exists = exports['devhub_licenses']:templateExists(templateName)
```

* **What it does**: Cheap existence check — useful for validating admin command input before calling `addLicense`.
* **Returns**: `boolean, boolean, string|nil` — `exists, success, error`. `true/false, true, nil` on a real check, or `false, false, "missing_arguments"` if no name was passed.

#### getDefaultLicenses

```lua
local names = exports['devhub_licenses']:getDefaultLicenses()
```

* **What it does**: Returns the template names configured to be issued automatically to new players.
* **Returns**: `table, boolean, string|nil` — array of template names; always `success = true`.

#### getFakeableLicenses

```lua
local names = exports['devhub_licenses']:getFakeableLicenses()
```

* **What it does**: Returns the template names that can be forged as fake IDs.
* **Returns**: `table, boolean, string|nil` — array of template names; always `success = true`.

#### getDataFields

```lua
local fields = exports['devhub_licenses']:getDataFields()
```

* **What it does**: Returns the configured `Config.DataFields` structure — the fields printed on licenses (name, date of birth, blood type…).
* **Returns**: `table, boolean, string|nil` — array of `{ name, label, default, showInProfile, liveProfileDisplay }`; always `success = true`.

#### getPlayerLiveData

```lua
local data = exports['devhub_licenses']:getPlayerLiveData(source)
```

* **What it does**: Runs each data field's `getData()` function for the player and returns the current values. Use it to preview what a freshly issued license would print. If a field's function errors, its configured `default` is returned instead.
* **Returns**: `table, boolean, string|nil` — map of field name → value with `success = true`, or empty `{}` with `error = "invalid_source"`.

### Session helpers

#### isPlayerLoaded

```lua
local loaded = exports['devhub_licenses']:isPlayerLoaded(source)
```

* **What it does**: Reports whether the player's license data has finished loading. Check this before acting on a player who just connected.
* **Returns**: `boolean, boolean, string|nil` — `loaded, success, error`. `true/false, true, nil` on a real check, or `false, false, "invalid_source"` if the server ID was missing or has no identifier.

#### getSourceByIdentifier

```lua
local src = exports['devhub_licenses']:getSourceByIdentifier(identifier)
```

* **What it does**: Resolves an identifier back to a server ID.
* **Returns**: `number|nil, boolean, string|nil` — the server ID with `success = true`, or `nil` with `error = "missing_arguments"` (no identifier) / `"player_not_loaded"` (that player is offline).

#### getLoadedPlayers

```lua
local identifiers = exports['devhub_licenses']:getLoadedPlayers()
```

* **Returns**: `table, boolean, string|nil` — array of identifiers of every player currently loaded in the license system; always `success = true`.

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

## <mark style="color:yellow;">**Framework Compatibility (esx_license / QBCore / Qbox)**</mark>

Since v1.2.0 the License System ships a compatibility layer (configured in
[`configs/s.compat.lua`](configuration/s.compat.lua.md)) that lets scripts written for other license
systems keep working unmodified.

### esx_license impersonation

With `Config.Compat.EsxLicense.enabled = true` (and no real `esx_license` resource running), the
License System answers the **entire esx_license API**. Existing ESX scripts need no changes:

| Surface | Names | Behavior |
| ------- | ----- | -------- |
| **Net events** | `esx_license:addLicense(target, type, cb)` · `esx_license:removeLicense(target, type, cb)` | Add issues the mapped template (grant + record, no photo); remove revokes the grant* and every owned record and takes the physical card. `cb` is optional and **always invoked**, even on failure — consumers can never hang. |
| **Server events** | `esx_license:checkLicense(target, type, cb)` · `esx_license:getLicenses(target, cb)` · `esx_license:getLicense(type, cb)` · `esx_license:getLicensesList(cb)` | Trigger server-side with an inline callback, exactly like the original: `TriggerEvent('esx_license:checkLicense', src, 'weapon', function(hasIt) ... end)`. `getLicenses` returns the familiar array of `{ type, label }`. |
| **ESX callbacks** | `esx_license:checkLicense` · `getLicenses` · `getLicense` · `getLicensesList` | Registered with es\_extended, so `ESX.TriggerServerCallback('esx_license:checkLicense', cb, target, type)` works from any client script. |
| **SQL tables** | `licenses` · `user_licenses` | With `sqlMirror = true` the classic tables are created and kept in sync, so scripts that `SELECT` them directly (ox\_inventory's ESX bridge, car dealers) keep working. |

\* Default licenses keep their grant (they are irrevocable everywhere in the script) — the records
are still revoked, so `checkLicense` turns `false`.

License **types** translate through `Config.Compat.EsxLicense.typeMap` (`drive` →
`driving_license`, `weapon` → `weapon_license`, …). A check against the impersonated
`esx_license:checkLicense` follows the same rule as `playerHasLicense`: `true` only for an
**active**, owned, non-fake license — so a suspension issued in the MDT correctly fails esx-side
checks, and esx flows cannot re-issue a suspended license.

{% hint style="warning" %}
Security differences from the original resource (both configurable): client-triggered
`esx_license:addLicense` is **blocked by default** (`allowClientAddLicense = false` — in vanilla
esx\_license any cheater could self-grant licenses), and client-triggered
`esx_license:removeLicense` requires a job from `removeLicenseJobs` (default: `police`, matching
esx\_policejob). Server-to-server calls are never restricted.
{% endhint %}

### QBCore / Qbox metadata mirror

With `Config.Compat.QbLicences.enabled = true`, the flags in `PlayerData.metadata.licences`
(`driver`, `weapon`, `id`, …) mirror the License System according to
`Config.Compat.QbLicences.map`, updating on every license change and on login. Stock qb-cityhall,
qb-policejob, qb-shops, qb-phone and ox\_inventory's QB bridge read exactly these flags. The mirror
is **one-way** — see the [config page](configuration/s.compat.lua.md) for details and caveats.

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
