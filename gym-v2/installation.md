---
description: How to install DevHub Gym V2 on your server.
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

Download the <mark style="color:red;">GYM V2</mark> files from keymaster. The product ships **three** resources:

| Resource | What it is |
| --- | --- |
| `devhub_gym2` | The script itself. |
| `devhub_gym2_assets` | Streamed gym props (the machines you place in the creator). |
| `devhub_gym2_assets_anim` | Streamed animation props used by the exercises. |

All three are required. The two asset resources contain no code, only stream files.
{% endstep %}

{% step %}
### Add the supplement items to your inventory

The resource ships an `items/` folder with an `items.lua` file and the item images.

* Open `items/items.lua`. It contains a ready-made block for **qb-core** (`qb-core/shared/items.lua`) and one for **ox\_inventory** (`ox_inventory/data/items.lua`). Copy the block that matches your inventory. On ESX with a database `items` table, add the eight item names by hand.
* Copy every `.png` from `items/imgs/` into your inventory's image folder (for example `qb-inventory/html/images` or `ox_inventory/web/images`).

The eight supplements are `gym_iron_surge`, `gym_pump_juice`, `gym_ripped_recovery`, `gym_hardcore_stack`, `gym_beast_mode_fuel`, `gym_apex_predator`, `gym_american_mass` and `gym_king_of_iron`.

{% hint style="warning" %}
An item that does not exist on your server can never be sold or used. If you rename one, rename it in the **Items** page of the Gym Creator too, and in the gym's **Warehouse** if you run the business module.
{% endhint %}
{% endstep %}

{% step %}
### Database

**You do not have to do anything here.** The script creates its own tables and inserts the default data the first time it starts.

If you prefer to run it yourself, import `sql/install.sql`. It is safe to run on a fresh **or** an existing database: every table uses `CREATE TABLE IF NOT EXISTS` and every seed row is only inserted when it is not already there, so running it twice never duplicates or overwrites anything.

The install creates a **ready-to-play gym at Vespucci Beach** with equipment, a map blip and interaction points already placed, so you can walk in and train the moment the server boots.
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```javascript
ensure devhub_lib
ensure devhub_gym2_assets
ensure devhub_gym2_assets_anim
ensure devhub_gym2
```

If you also run [devhub\_skillTree](https://store.devhub.gg/product/6438994), start it **before** `devhub_gym2`.
{% endstep %}

{% step %}
### Restart your server
{% endstep %}

{% step %}
### Open the creator in game

Log in as an admin and type `/admindevhub`, then pick **Gym** and **Gym Creator**.

Everything from here on is done in the interface. See [gym-creator](gym-creator/ "mention").
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
DO NOT CHANGE RESOURCE NAME
{% endhint %}

***

## <mark style="color:yellow;">**Who can open the admin menu**</mark>

The Gym Creator and the business admin panel are gated by the ACE permission `command`, the same one the DevHub library uses everywhere. Give it to your admin group in `server.cfg`:

```javascript
add_ace group.admin command allow
add_principal identifier.license:YOUR_LICENSE group.admin
```

A player without that permission cannot open the creator, and the server rejects every creator action even if the interface is forced open.

***

## <mark style="color:yellow;">**Optional: an MLO gym instead of the default one**</mark>

The resource ships ready-made layouts for five popular gym MLOs in `maps_plug_and_play/`. Each one is a small SQL file plus the map files it has to replace, so the whole gym appears already built inside that MLO.

{% content-ref url="map-bundles.md" %}
[map-bundles.md](map-bundles.md)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**Optional: the business module**</mark>

Gym V2 can run as a player-owned business (employees, wages, a warehouse, a shop, taxes, company levels). That part is a separate DevHub license and is **off by default**. Nothing else in the script depends on it.

{% content-ref url="businesses/" %}
[businesses](businesses/)
{% endcontent-ref %}

{% content-ref url="../scripts/devhub_lib-needed-for-each-script/" %}
[devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/)
{% endcontent-ref %}

{% content-ref url="../download-purchased-assets.md" %}
[download-purchased-assets.md](../download-purchased-assets.md)
{% endcontent-ref %}
