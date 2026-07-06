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

{% hint style="info" %}
License avatar photos are captured in-game and uploaded to an image host that **you** choose in
`configs/s.imagehost.lua`. Set this up before issuing licenses — the recommended path is a free
[uploadhub.gg](https://uploadhub.gg) webhook (permanent links). See the guide below.
{% endhint %}

{% content-ref url="avatar-photo-hosting.md" %}
[avatar-photo-hosting.md](avatar-photo-hosting.md)
{% endcontent-ref %}
{% endstep %}

{% step %}
### Database Setup

Import the `sql.sql` file into your database.
{% endstep %}

{% step %}
### Restart your server
{% endstep %}
{% endstepper %}



{% content-ref url="../scripts/devhub_lib-needed-for-each-script/" %}
[devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/)
{% endcontent-ref %}
