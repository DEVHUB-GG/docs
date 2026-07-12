---
description: >-
  Exports and open-function hooks exposed by the License System for integrating
  with other resources.
---

# ⚙️ Exports & Events

The License System exposes client and server **exports** plus two open-function tables —
`OpenClientFunctions` (in `configs/c.functions.lua`) and `OpenServerFunctions`
(in `configs/s.functions.lua`) — that you can edit to react to in-game events.

## <mark style="color:yellow;">**Client Exports**</mark>

Call these from any client script as `exports['devhub_licenses']:<name>(...)`.

```lua
exports['devhub_licenses']:openMdt(isLaptopApp)            -- Open the license MDT tablet
exports['devhub_licenses']:openCardHolder()                -- Open the local player's card holder
exports['devhub_licenses']:openCardHolderForPlayer(targetServerId) -- Open another player's card holder
exports['devhub_licenses']:openFavoriteHand()              -- Open the favorite licenses hand display

exports['devhub_licenses']:getMyLicenses()                 -- Licenses owned by the local player
exports['devhub_licenses']:getAllMyLicenses()              -- All licenses the local player carries
exports['devhub_licenses']:getLicenseById(licenseId)       -- A single carried license by ID
exports['devhub_licenses']:hasLicense(templateName)        -- bool, licenseId — active license of a template?
exports['devhub_licenses']:getLicenseCount(templateName)   -- Count owned (optionally by template)
exports['devhub_licenses']:getMyFavorites()                -- The local player's favorite licenses

exports['devhub_licenses']:showLicenseTerminal(callback)   -- Open a scan terminal with a scan callback
exports['devhub_licenses']:closeLicenseTerminal()          -- Close the scan terminal
```

* **openMdt(isLaptopApp)**: Opens the MDT; pass `true` when opening as a `devhub_laptop` app.
* **openCardHolderForPlayer(targetServerId)**: Opens the card holder for another player (police search, etc.); `targetServerId` is required.
* **hasLicense(templateName)**: Returns `true` plus the matching `licenseId` if the player has an **active** license of that template.
* **getLicenseCount(templateName)**: Counts carried licenses; omit `templateName` to count all.
* **showLicenseTerminal(callback)**: Opens a scan terminal. The `callback(license)` receives the scanned license and returns a result table such as `{ success = true, message = "Access Granted" }`. See `configs/c.terminal_demo.lua` for a full example.

***

## <mark style="color:yellow;">**Server Exports**</mark>

Call these from any server script as `exports['devhub_licenses']:<name>(...)`.

```lua
-- Reading data
exports['devhub_licenses']:getPlayerLicenses(source)
exports['devhub_licenses']:getPlayerOwnedLicenses(source)
exports['devhub_licenses']:getPlayerLicenseById(source, licenseId)
exports['devhub_licenses']:playerHasLicense(source, templateName)        -- bool, licenseId
exports['devhub_licenses']:playerHasGrant(source, templateName)          -- bool, status
exports['devhub_licenses']:getPlayerGrants(source)
exports['devhub_licenses']:getPlayerIdentifier(source)
exports['devhub_licenses']:getPlayerLicenseCount(source, templateName, status)

-- Mutating data
exports['devhub_licenses']:addLicense(source, templateName)
exports['devhub_licenses']:updateLicenseStatus(source, owner, licenseId, newStatus, reason, suspensionTime)
exports['devhub_licenses']:grantPermission(source, targetSource, templateName)
exports['devhub_licenses']:revokePermission(source, targetSource, templateName)
exports['devhub_licenses']:destroyLicense(source, owner, licenseId)
exports['devhub_licenses']:refreshPlayerLicenses(source)

-- Templates & metadata
exports['devhub_licenses']:getAllTemplates()
exports['devhub_licenses']:getTemplate(templateName)
exports['devhub_licenses']:templateExists(templateName)
exports['devhub_licenses']:getDefaultLicenses()      -- auto-issued template names
exports['devhub_licenses']:getFakeableLicenses()     -- fakeable template names
exports['devhub_licenses']:getDataFields()
exports['devhub_licenses']:getPlayerLiveData(source)

-- Player session helpers
exports['devhub_licenses']:isPlayerLoaded(source)
exports['devhub_licenses']:getSourceByIdentifier(identifier)
exports['devhub_licenses']:getLoadedPlayers()
```

* **updateLicenseStatus(...)**: `newStatus` must be one of `active`, `suspended`, `revoked`, `lost`, `expired`. `owner` is the license owner's identifier; `suspensionTime` is a Unix timestamp for when a suspension ends.
* **destroyLicense(source, owner, licenseId)**: Permanently removes the license and fires the `OnLicenseDestroyed` hook.
* **getPlayerLicenseCount(source, templateName, status)**: Both `templateName` and `status` are optional filters.
* **getPlayerLiveData(source)**: Runs each `DataFields` `getData()` function for the player and returns the live values.

***

## <mark style="color:yellow;">**Client Open Functions**</mark>

Edit the `OpenClientFunctions` table in `configs/c.functions.lua` to react to client-side events.
Functions whose comment says "Return false to …" can cancel the action by returning `false`.

```lua
OpenClientFunctions = {
    OnMdtOpened = function(isLaptopApp) end,
    OnMdtClosed = function() end,
    OnCardHolderOpened = function(licenseCount) end,
    OnCardHolderClosed = function() end,
    OnFavoriteHandOpened = function(favoriteCount) end,
    OnFavoriteHandClosed = function() end,
    OnShowLicense = function(licenseId) return true end,             -- return false to block
    OnLicenseDisplayReceived = function(templateData, licenseData, sourceServerId) end,
    OnNewLicensePreview = function(previewData) end,
    OnCheckPlayerLicenses = function(targetServerId, targetEntity) return true end, -- return false to cancel
    OnViewingPlayerLicenses = function(targetServerId, playerName, licenseCount) end,
    OnTerminalOpened = function() end,
    OnLicenseScanned = function(licenseId, templateName, licenseData) end,
    OnTerminalClosed = function() end,
    OnScreenshotStarted = function() end,
    OnScreenshotCompleted = function(imageUrl) end,
    OnHideoutDoorHandler = function(entity) return true end,         -- return false to hide option
    OnHideoutCarpetHandler = function(entity, distance, coords) return true end,
}
```

{% hint style="info" %}
`OnHideoutDoorHandler` and `OnHideoutCarpetHandler` are called **repeatedly** by the target system to decide whether to show the interaction option — not once on interaction. Return `true` to show it, `false` to hide it.
{% endhint %}

***

## <mark style="color:yellow;">**Server Open Functions**</mark>

Edit the `OpenServerFunctions` table in `configs/s.functions.lua` to react to server-side events.
By default each hook sends a Discord log via `Config.LogsWebhook`. Hooks documented as "Return
false to …" can block the action.

```lua
OpenServerFunctions = {
    OnLicenseCreated = function(source, licenseId, templateName, templateLabel, licenseData, expirationDate) end,
    OnFakeLicenseCreated = function(source, licenseId, templateName, templateLabel, fakeData) end,
    OnLicenseStatusChanged = function(source, owner, licenseId, templateName, oldStatus, newStatus, reason, suspensionTime) end,
    OnLicenseDestroyed = function(source, licenseId, templateName, identifier) end,
    OnLicenseGiven = function(source, targetSource, licenseId, templateName, giverIdentifier, receiverIdentifier) end,
    OnLicenseTaken = function(source, targetSource, licenseId, templateName, takerIdentifier, victimIdentifier) end,
    OnPermissionGranted = function(source, identifier, templateName, templateLabel) return true end,  -- return false to block
    OnPermissionRevoked = function(source, identifier, templateName, templateLabel) end,
    OnTemplateSaved = function(source, templateName, templateData, isUpdate) end,
    OnPlayerLoaded = function(source, identifier, licenses, granted) end,
    OnPlayerUnloaded = function(source, identifier) end,
    OnLicenseShown = function(source, licenseId, templateName, targetServerId) return true end,       -- return false to block
    OnPlayerLicensesChecked = function(source, targetServerId) return true end,                       -- return false to block
    OnFakeIdMissionCompleted = function(source, licenseId, templateName, payment) end,
    CanPickupLicense = function(source, templateName, paymentMethod, chargeAmount) return true end,    -- return false to block
    OnLicensePickedUp = function(source, templateName, paymentMethod, chargeAmount) end,
}
```

* **CanPickupLicense**: Run custom checks before a player picks up a license at the city hall NPC — e.g. require passing a driving test. Return `false` to deny.
* **OnPlayerLicensesChecked**: Gate license checks behind a job (e.g. police only) by returning `false` for unauthorized players.
* **OnLicenseStatusChanged**: `source` may be `nil` when the change is automatic (system/offline), such as an expiration or suspension ending.
