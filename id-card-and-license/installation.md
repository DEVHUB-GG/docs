# 💻 Installation

{% stepper %}
{% step %}
### Install devhub\_lib

Download and install the required library and configure it according to your framework.

Download [https://github.com/DEVHUB-GG/devhub\_lib ](https://github.com/DEVHUB-GG/devhub_lib)or use command

```bash
git clone https://github.com/DEVHUB-GG/devhub_lib.git
```
{% endstep %}

{% step %}
### Install screenshot-basic

`screenshot-basic` is **required** — the license system uses it to capture and upload the player's
license portrait. It ships with most servers in the default `[system]` resources; if yours doesn't
have it, download it from [citizenfx/screenshot-basic](https://github.com/citizenfx/screenshot-basic)
and add it to your `resources` folder.
{% endstep %}

{% step %}
### Install resources from keymaster

Download the <mark style="color:red;">LICENSE SYSTEM</mark> script file from keymaster.
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```javascript
ensure devhub_lib
ensure screenshot-basic
ensure devhub_licenses
```
{% endstep %}

{% step %}
### Database Setup

Import the `sql.sql` file into your database.
{% endstep %}

{% step %}
### Set up avatar photo hosting

<mark style="color:red;">**Required.**</mark> License photos are captured in-game and uploaded to an
image host that **you** choose in `configs/s.imagehost.lua`. Configure it **before** issuing any
license, otherwise licenses are created without a picture.

Open `configs/s.imagehost.lua` and either:

* keep `provider = "webhook"` and paste your own [uploadhub.gg](https://uploadhub.gg) webhook URL
  into the `webhook` field (recommended, the links never expire), or
* set `provider = "custom"` and host the images yourself through the `UploadAvatarImage` function.

```lua
Config.AvatarUpload = {
    provider = "webhook",
    webhook = "YOUR_UPLOADHUB_WEBHOOK_URL",
}
```

{% hint style="danger" %}
Do not paste a raw Discord webhook here. Discord CDN links expire, so every license photo will
eventually turn into a broken image.
{% endhint %}

Full step-by-step guide:
[https://devhub.gg/docs/id-card-and-license/avatar-photo-hosting](https://devhub.gg/docs/id-card-and-license/avatar-photo-hosting)

{% content-ref url="avatar-photo-hosting.md" %}
[avatar-photo-hosting.md](avatar-photo-hosting.md)
{% endcontent-ref %}
{% endstep %}

{% step %}
### Add the items to your inventory

Register the items shipped in the resource's `items/` folder (holder, favorites, MDT, fake-license
supplies, and any physical license items) in your inventory resource, and copy the icons from
`items/images`.

{% content-ref url="using-licenses-as-items.md" %}
[using-licenses-as-items.md](using-licenses-as-items.md)
{% endcontent-ref %}
{% endstep %}

{% step %}
### Restart your server
{% endstep %}
{% endstepper %}



{% content-ref url="../scripts/devhub_lib-needed-for-each-script/" %}
[devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/)
{% endcontent-ref %}
