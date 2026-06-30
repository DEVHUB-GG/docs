# 💻 Installation

{% stepper %}
{% step %}
### Install [devhub\_lib](https://docs.devhub.gg/scripts/devhub_lib-needed-for-each-script)

Download and install the required library and [configure](https://docs.devhub.gg/scripts/devhub_lib-needed-for-each-script) it according to your framework.

Download [https://github.com/DEVHUB-GG/devhub\_lib ](https://github.com/DEVHUB-GG/devhub_lib)or use command

```bash
git clone https://github.com/DEVHUB-GG/devhub_lib.git
```
{% endstep %}

{% step %}
### Install devhub\_grid

Train Heist uses **devhub\_grid** for the in-vehicle trunk grid that the looted cargo is loaded into. Install it from your DevHub keymaster and ensure it **before** `devhub_trainHeist`.
{% endstep %}

{% step %}
### Ensure oxmysql

Train Heist depends on `oxmysql`. It is a base resource on virtually every server, so it is normally already running — just make sure it is ensured before `devhub_trainHeist`.
{% endstep %}

{% step %}
### Install resource from keymaster

* Download the <mark style="color:red;">TRAIN HEIST</mark> script file from keymaster.
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```javascript
ensure oxmysql
ensure devhub_lib
ensure devhub_grid
ensure devhub_trainHeist
```
{% endstep %}

{% step %}
### \[OPTIONAL] Add the devhub\_laptop Crime Traders integration

Train Heist registers itself as a **Crime Traders** task in the devhub\_laptop. If you use the laptop, ensure it **before** `devhub_trainHeist` so the app loads first:

```javascript
ensure oxmysql
ensure devhub_lib
ensure devhub_grid
ensure devhub_laptop
ensure devhub_trainHeist
```

The integration is toggled with `Shared.LaptopConfig.enabled` in `configs/shared.lua`. Players can also join the queue by talking to the in-world Rail Network Contact NPC, so the laptop is not strictly required.

[https://devhub.gg/product/7054272](https://devhub.gg/product/7054272)
{% endstep %}

{% step %}
### \[OPTIONAL] Add the devhub\_skillTree integration

Train Heist can award XP and apply skill perks through the **Crime Traders** skill tree in the laptop ([devhub\_skillTree](https://store.devhub.gg/product/6438994)). Enable it with `Shared.SkillTree.enabled` and ensure `devhub_skillTree` before `devhub_trainHeist`. Without it everything below the skill tree quietly no-ops.
{% endstep %}

{% step %}
### Restart your server
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
