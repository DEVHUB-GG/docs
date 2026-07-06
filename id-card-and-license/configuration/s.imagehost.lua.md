---
description: Documentation for s.imagehost.lua Configuration
---

# s.imagehost.lua

This file controls **where license avatar photos are hosted**. When a player picks up a license the
script captures their portrait in-game; this file decides where that image is uploaded and stored so
it can be shown on the license and in profiles.

{% hint style="info" %}
Looking for the step-by-step webhook setup with screenshots? See
[🖼️ Avatar Photo Hosting](../avatar-photo-hosting.md).
{% endhint %}

## <mark style="color:yellow;">**AvatarUpload**</mark>

```lua
Config.AvatarUpload = {
    -- "webhook" | "custom"
    provider = "webhook",

    -- Webhook URL (Discord format). Use an https://uploadhub.gg webhook so the
    -- links never expire. Only used when provider = "webhook".
    webhook = "",
}
```

* **Description**: Chooses how captured license photos are hosted.
* **provider**: Hosting path.
  * `"webhook"` _(default)_ — upload the photo to the Discord-format `webhook` URL below.
  * `"custom"` — host the photo yourself via the `UploadAvatarImage` hook (see below). Any value
    other than `"webhook"` routes here.
* **webhook**: Discord-format webhook URL used when `provider = "webhook"`. <mark style="color:red;">Do not use a raw Discord webhook — its links expire and photos will break.</mark> Use an [uploadhub.gg](https://uploadhub.gg) webhook instead. Leave `""` to disable webhook hosting.

{% hint style="warning" %}
Keep the webhook URL **server-side only** — this file is a server config (`s.*.lua`) and is never sent to clients.
{% endhint %}

***

## <mark style="color:yellow;">**UploadAvatarImage** (custom host)</mark>

```lua
function UploadAvatarImage(source, base64, format, resolve)
    -- Upload the image wherever you like, then call resolve(publicUrl) with the
    -- public URL — or resolve(nil) if it failed. The upload can be async.
    resolve(nil)
end
```

* **Description**: Server-side hook used when `provider = "custom"` (or any non-`"webhook"` provider). Upload the captured image to your own host, then hand its public URL back through `resolve`. The upload may be async — call `resolve()` whenever you're done.
* **source**: Server id of the player the photo belongs to.
* **base64**: Raw base64 image data (no `data:` prefix).
* **format**: Image format/extension — `"jpg"`, `"png"`, or `"webp"`.
* **resolve**: `fun(url: string|nil)` callback — pass the hosted public URL, or `nil` on failure.

{% hint style="info" %}
The file ships with a commented **Imgur** example showing the exact shape of a custom uploader. Adapt it (or swap in S3, your own CDN, etc.) and return the public URL via `resolve()`.
{% endhint %}
