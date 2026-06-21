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
### Install resource from keymaster

* Download the <mark style="color:red;">CAR THIEF</mark> script file from keymaster.
{% endstep %}

{% step %}
### Add items from the `items` folder

Open the `items` folder and add the two items to your inventory resource (a ready-made snippet for `qb-core`, `ox_inventory` and SQL inventories is included):

* `devhub_car_checker` — **Car Checker**
* `devhub_key_cloner` — **Key Cloner**

Copy the item images from the `items` folder into your inventory's image folder.
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```javascript
ensure devhub_lib
ensure devhub_carThief
```
{% endstep %}

{% step %}
### \[OPTIONAL] Add the devhub\_laptop Crime Traders integration

Car Thief registers itself as a **Crime Traders** task in the devhub\_laptop. If you use the laptop, ensure it **before** `devhub_carThief` so the app loads first:

```javascript
ensure devhub_lib
ensure devhub_laptop
ensure devhub_carThief
```

The integration is toggled with `Config.LaptopConfig.enabled`. Players can also join the queue by talking to the in-world Car Meet Contact NPC, so the laptop is not strictly required.

[https://devhub.gg/product/7054272](https://devhub.gg/product/7054272)
{% endstep %}

{% step %}
### \[OPTIONAL] Add the devhub\_skillTree integration

Car Thief can award XP and apply skill perks through [devhub\_skillTree](https://store.devhub.gg/product/6438994). Enable it with `Config.SkillTree.enabled` and ensure `devhub_skillTree` before `devhub_carThief`.
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
