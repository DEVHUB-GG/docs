---
description: Documentation for s.main.lua Configuration
---

# s.main.lua

This file is server-only. It holds the Discord logging webhook, the avatar photo hosting
settings, and the license **data fields**.

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

## <mark style="color:yellow;">**Screenshot**</mark>

```lua
Config.Screenshot = {
    -- Photo hosting method:
    --   "local" -> save it on the server and serve it over the built-in image API
    method = "local",

    ["local"] = {
        folder = "uploads", -- folder inside this resource where photos are stored
        format = "webp",    -- image format for saved photos: "webp", "jpg" or "png"

        -- Crop the captured frame to a centered portrait before saving.
        -- Values are fractions (0..1) of the full screenshot: x/y = top-left corner,
        -- width/height = size. Set crop = false to store the full frame.
        crop = {
            x = 0.22,
            y = 0.0,
            width = 0.56,
            height = 1.0,
        },

        -- Resize the (cropped) photo to a fixed resolution before saving.
        -- Set size = false to keep the cropped resolution.
        size = {
            width = 312,
            height = 312,
        },
    },
}
```

* **Description**: Controls how the license avatar photo (captured in‑game) is hosted. Photos are stored on **your** server and served on demand by the script's built‑in image API — no external service (Discord, Imgur, etc.) is involved.
* **method**: Hosting method. Only `"local"` is available.
* **local**: Settings for locally‑hosted photos.
  * **folder**: Folder name used for stored photos.
  * **format**: Image format for saved photos — `"webp"`, `"jpg"`, or `"png"`. `webp` produces the smallest files.
  * **crop**: Crop the captured frame to a centered portrait before saving. Values are fractions (`0`–`1`) of the full screenshot: `x`/`y` is the top‑left corner, `width`/`height` the size. Set `crop = false` to store the full frame.
  * **size**: Resize the cropped photo to a fixed resolution (`width` × `height`, in pixels). Set `size = false` to keep the cropped resolution.

{% hint style="info" %}
**No external image host required.** The script ships a small built‑in image API that stores avatars on your server and serves them over HTTP only when they're actually shown. Players **do not** download stored photos when they join — each avatar is fetched on demand the first time it's displayed and then cached. The API's dependencies are installed automatically on first start (see [Installation](../installation.md)).
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
