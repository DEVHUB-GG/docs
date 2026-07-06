---
description: >-
  How license avatar photos are captured, uploaded, and saved — plus a
  step-by-step guide to the recommended uploadhub.gg webhook host.
---

# 🖼️ Avatar Photo Hosting

When a player picks up a license, the script captures their character portrait in-game and needs
somewhere permanent to store that image so it can be shown on the license, in the MDT, and in
player profiles. The License System hands this off to an image host that **you** choose in
[`configs/s.imagehost.lua`](configuration/s.imagehost.lua.md).

There are two hosting paths:

* **`webhook`** _(recommended, default)_ — the photo is uploaded to a Discord-format webhook URL.
  Use an [uploadhub.gg](https://uploadhub.gg) webhook (see below) so the links never expire.
* **`custom`** — you host the image yourself (Imgur, S3, your own CDN, …) via the
  `UploadAvatarImage` hook. See [s.imagehost.lua](configuration/s.imagehost.lua.md).

{% hint style="danger" %}
**Do not paste a raw Discord webhook here.** Discord's CDN links now expire after a while, so every
license photo will eventually turn into a broken image. Use an [uploadhub.gg](https://uploadhub.gg)
webhook instead — it gives you a Discord-format URL but hosts the images **permanently**.
{% endhint %}

## <mark style="color:yellow;">**Recommended: uploadhub.gg**</mark>

[uploadhub.gg](https://uploadhub.gg) hands you a Discord-format webhook URL while hosting every
uploaded image permanently (links never expire). Set it up in a few minutes:

{% stepper %}
{% step %}
### Create a new project

Visit [https://uploadhub.gg](https://uploadhub.gg), log in, and create a new project.

<div data-full-width="false"><figure><img src="https://upload.devhub.gg/dh_upload/uploadhub/createNewProject.png" alt=""><figcaption></figcaption></figure></div>
{% endstep %}

{% step %}
### Generate a Discord-format webhook

Open the **Webhook** tab and generate a new webhook with the format set to **Discord**.

<div data-full-width="false"><figure><img src="https://upload.devhub.gg/dh_upload/uploadhub/createWebhook.png" alt=""><figcaption></figcaption></figure></div>
{% endstep %}

{% step %}
### Copy the webhook URL

Copy the generated webhook URL.

<div data-full-width="false"><figure><img src="https://upload.devhub.gg/dh_upload/uploadhub/copyWebhook.png" alt=""><figcaption></figcaption></figure></div>
{% endstep %}

{% step %}
### Paste it into the config

Open `configs/s.imagehost.lua`, keep `provider = "webhook"`, and paste the URL into the `webhook`
field.

<div data-full-width="false"><figure><img src="https://upload.devhub.gg/dh_upload/uploadhub/addWebhookToLua.png" alt=""><figcaption></figcaption></figure></div>

```lua
Config.AvatarUpload = {
    provider = "webhook",
    webhook = "YOUR_UPLOADHUB_WEBHOOK_URL",
}
```
{% endstep %}

{% step %}
### Restart the resource

Restart `devhub_licenses`. New license photos will now upload to your uploadhub.gg project and stay
online permanently.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Prefer to host images on your own infrastructure (Imgur, S3, a private CDN)? Set
`provider = "custom"` and implement the `UploadAvatarImage` hook instead — see the config page below.
{% endhint %}

{% content-ref url="configuration/s.imagehost.lua.md" %}
[s.imagehost.lua](configuration/s.imagehost.lua.md)
{% endcontent-ref %}
