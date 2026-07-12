---
description: How to install DevHub Petty Crimes on your server.
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

* Download the <mark style="color:red;">PETTY CRIMES</mark> script file from keymaster.
{% endstep %}

{% step %}
### Add the items to your inventory

The resource ships an `items/` folder with an `items.txt` file and the item images.

* Open `items/items.txt` — it contains ready-made blocks for **qb-core** (`qb-core/shared/items.lua`), **ox\_inventory** (`ox_inventory/data/items.lua`) and **ESX** (an `INSERT` for the `items` table). Copy the block that matches your inventory.
* Copy every `.png` from the `items/` folder into your inventory's image folder (for example `qb-inventory/html/images` or `ox_inventory/web/images`).

{% hint style="warning" %}
Items that do not exist on your server can never drop. If you rename an item, also change it in the **Rewards** tab of the in-game editor and in the **Shop** editor.
{% endhint %}
{% endstep %}

{% step %}
### Database Setup

Import the `sql.sql` file into your database.

Besides creating the table, it also inserts the **default configuration of every activity and of the shop**, so the script is fully playable the moment you start it.
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```javascript
ensure devhub_lib
ensure devhub_pettyCrimes
```
{% endstep %}

{% step %}
### Restart your server
{% endstep %}

{% step %}
### Configure the script in-game

Log in as an admin and type `/pettyCrimes` (or open `/admindevhub`) to enable the activities you want, place their coords and set up the dealer.

See [script-flow.md](script-flow.md "mention") for what each activity does.
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
