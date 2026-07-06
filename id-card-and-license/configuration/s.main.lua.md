---
description: Documentation for s.main.lua Configuration
---

# s.main.lua

This file is server-only. It holds the Discord logging webhook and the license **data fields**.

{% hint style="info" %}
Avatar photo hosting has moved to its own file — see [s.imagehost.lua](s.imagehost.lua.md) and the
[🖼️ Avatar Photo Hosting](../avatar-photo-hosting.md) guide.
{% endhint %}

## <mark style="color:yellow;">**LogsWebhook**</mark>

```lua
-- Discord Webhook for logging all license activities
-- Set to false or "" to disable logging
Config.LogsWebhook = "https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE"
```

* **Description**: Discord webhook URL that receives logs for every license activity (creation, status changes, gives/takes, missions, etc.). Set to `false` or `""` to disable logging.

{% hint style="warning" %}
Keep this webhook **server-side only** — never expose it to clients. The client triggers logs via the `devhub_licenses:server:sendLog` event so the URL stays on the server.
{% endhint %}

***

## <mark style="color:yellow;">**DataFields**</mark>

```lua
Config.DataFields = {
    {
        name = 'fullName',
        label = 'Full Name',
        default = 'John Doe',
        getData = function(source)
            if not Core.GetFullName then return "Unknown" end
            return Core.GetFullName(source)
        end,
        showInProfile = true,
        liveProfileDisplay = true,
    },
    -- firstName, lastName, dateOfBirth, Sex, Nationality, height, jobLabel,
    -- jobGrade, issueDate, expirationDate, licenseNumber, skinColor, eyeColor ...
}
```

* **Description**: Defines every data field available to place on a license template and to show in player profiles. Each field is a table with the following keys:
  * **name**: Unique key used in code and bound to license components.
  * **label**: Human-readable label shown in the UI.
  * **default**: Fallback value used when no data is available.
  * **getData**: `function(source, other)` returning the live value for the field. Receives the player `source`; date/number fields (`issueDate`, `expirationDate`, `licenseNumber`) also receive the license record as `other`.
  * **showInProfile**: When `true`, the field is shown in the profile view.
  * **liveProfileDisplay**: When `true`, for online players the value is fetched fresh via `getData`; otherwise it comes from the player's latest issued license. <mark style="color:red;">Use only for data that changes often or is important (e.g. job).</mark>
  * **fakeFields**: Array of values offered as choices when forging a fake ID for this field.

{% hint style="info" %}
Several `getData` functions ship returning hard-coded demo values (e.g. date of birth, sex, height, job) with the real framework lookups commented out. Uncomment and adapt those lines to pull live data from your framework via `devhub_lib`'s `Core` helpers.
{% endhint %}
