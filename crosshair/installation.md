---
description: How to install DevHub Crosshair Maker on your server.
---

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
### Install resources from keymaster

* Download the <mark style="color:red;">CROSSHAIR MAKER</mark> script file from keymaster.
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```javascript
ensure devhub_lib
ensure devhub_crosshair
```

There is no database step: the script needs no SQL. A player's crosshair, profiles and zoom are saved on their own machine, and your admin settings are saved to `data/admin.json` inside the resource.
{% endstep %}

{% step %}
### Give yourself access to the admin panel

The panel is opened by an admin only. Any one of these is enough, and you can combine them (see [sh.config.lua.md](configuration/sh.config.lua.md "mention")):

* You are an admin of **devhub\_lib** (on by default).
* You have the ACE permission. Add this line to your `server.cfg`:

```bash
add_ace group.admin devhub_crosshair.admin allow
```

* Your identifier is listed in `Config.Admin.identifiers`.
{% endstep %}

{% step %}
### Restart your server
{% endstep %}

{% step %}
### Set the script up in-game

Log in as an admin and type `/crosshairadmin`. Everything else is configured there: the features, the allowed shapes and scopes, the slider limits, the weapons, the command and the keys. Press **Save & apply** and it is live for every player instantly.

{% hint style="warning" %}
The panel also shows up as **Crosshair** inside devhub\_lib's `/admindevhub` hub, but that hub has an admin check of its own: it only opens for a **devhub\_lib** admin. If you gave yourself access with the ACE permission or an identifier instead, use `/crosshairadmin`, which honours all three checks.
{% endhint %}

{% hint style="info" %}
The crosshair ships **off**. A player sees GTA's own reticle until they switch the custom one on with the toggle key (**F9** by default) or in the maker.
{% endhint %}

See [script-flow.md](script-flow.md "mention") for what players and admins can actually do.
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
DO NOT CHANGE RESOURCE NAME
{% endhint %}

{% content-ref url="../scripts/devhub_lib-needed-for-each-script/" %}
[devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/)
{% endcontent-ref %}

{% content-ref url="../download-purchased-assets.md" %}
[download-purchased-assets.md](../download-purchased-assets.md)
{% endcontent-ref %}
