---
description: >-
  How to register the License System items in your inventory, open the
  interfaces with a usable item, and turn licenses into real physical items.
---

# 📦 Using Licenses as Items

The License System can be used entirely with chat commands, but every interface can also be opened
from an inventory item, and licenses themselves can be turned into **physical items** that players
carry, hand over, and lose.

There are two separate things on this page:

* **Interface items** (holder, favorites, MDT). Always available. Using the item opens the matching
  interface.
* **Physical licenses** (`Config.UsePhysicalLicenses`). Optional. Each license template gets its own
  inventory item, and the license itself lives in the player's inventory instead of the virtual card
  holder.

***

## <mark style="color:yellow;">**Step 1: Register the items**</mark>

The resource ships everything you need in its `items/` folder:

* `items/items.txt` - ready-made item definitions for `qb-core/shared/items.lua`,
  `ox_inventory/data/items.lua`, and an ESX `INSERT INTO items` SQL statement.
* `items/images` - the item icons.

{% stepper %}
{% step %}
### Copy the item definitions

Open `items/items.txt`, take the block that matches your inventory, and paste it into your
inventory's item list (or run the SQL statement for ESX databases).
{% endstep %}

{% step %}
### Copy the icons

Copy every `.png` from `items/images` into your inventory's image folder (for example
`ox_inventory/web/images` or `qb-inventory/html/images`).
{% endstep %}

{% step %}
### Restart

Restart your inventory resource so the new items are loaded.
{% endstep %}
{% endstepper %}

### Shipped items

| Item | What it is |
| --- | --- |
| `devhub_licenseholder` | Opens the card holder (the licenses the player carries). |
| `devhub_favlicenses` | Opens the favorites hand. |
| `devhub_licensesmdt` | Opens the License MDT. |
| `devhub_fakelicenses` | Fake-license supply, consumed by the Paper Cutter. |
| `devhub_fakelicense` | Fake-license supply, consumed by the Foil Tipper. |
| `devhub_plasticfakelicense` | Fake-license supply, consumed by the Embosser. |
| `devhub_id_card`, `devhub_driving_license`, `devhub_weapon_license`, `devhub_medical_license`, `devhub_police_license`, `devhub_mechanic_license`, `devhub_builder_license` | Example physical license items, one per license template. |

The fake-license supplies are **not** usable items. They are ingredients consumed at the crafting
stations in the hideout, and players buy them from the underground NPC
(`Config.UndergroundItemShop`). The holder, favorites, and MDT items are sold by the License Pickup
NPC (`Config.ItemShop`).

***

## <mark style="color:yellow;">**Step 2: Open an interface with an item**</mark>

Each interface is bound to an item in `Config.Usage` in
[`configs/sh.main.lua`](configuration/sh.main.lua.md). The `item` field is the item name that opens
it, and `false` disables that trigger.

```lua
Config.Usage = {
    favorites = {
        command = "favorites",
        item    = "devhub_favlicenses",     -- usable item name (false to disable)
        event   = "devhub_licenses:client:openFavorites",
        keybind = false,
    },
    licenses = {
        command = "licenses",
        item    = "devhub_licenseholder",
        event   = "devhub_licenses:client:openLicenses",
        keybind = false,
    },
    licenseMdt = {
        command = "licenseMdt",
        item    = "devhub_licensesmdt",
        event   = "devhub_licenses:client:openMainMenu",
        keybind = false,
    },
}
```

To use your own item names, change the `item` values here to items that already exist in your
inventory. To make an interface item-only, set its `command` to `false`.

{% hint style="info" %}
Item names are case-sensitive and must match your inventory exactly. If nothing happens when the
item is used, the name in `Config.Usage` does not match a registered item.
{% endhint %}

***

## <mark style="color:yellow;">**Step 3: Turn licenses into physical items**</mark>

With physical licenses enabled, issuing a license also puts an inventory item in the player's
pockets. Using that item pulls the license out and shows it to nearby players, exactly like the
**Show** option in the card holder.

{% hint style="danger" %}
Enabling physical licenses **disables the virtual card holder**. The `/licenses` and `/favorites`
commands stop working, and players use their license items instead.
{% endhint %}

{% stepper %}
{% step %}
### Enable the feature

In [`configs/sh.main.lua`](configuration/sh.main.lua.md):

```lua
Config.UsePhysicalLicenses = true
```
{% endstep %}

{% step %}
### Create one item per license

Every license template that should exist as an item needs its own inventory item. Use the ready-made
ones from `items/items.txt` (`devhub_driving_license`, `devhub_id_card`, and so on) or add your own.

The items must be **unique / non-stacking** so each license keeps its own data.
{% endstep %}

{% step %}
### Link the item to the license template

Open the **License MDT**, open the **License Creator**, and edit the template. Under
**General → Item Integration**, type the item name into the **Item Name** field and save.

The panel shows an **Enabled / Disabled** badge that reflects `Config.UsePhysicalLicenses`. If it
reads **Disabled**, the item name is stored but not used yet.
{% endstep %}

{% step %}
### Restart the resource

Restart `devhub_licenses` after linking an item to an existing template so the item handler is
registered.
{% endstep %}
{% endstepper %}

### What players see

* **Getting a license** at the pickup NPC also gives the matching item, with the license data
  attached to it.
* **Using the item** plays the hold-up animation and shows that license to nearby players.
* **Losing the license** works both ways: reporting it lost, or having it revoked, removes the item
  from the player's inventory.
* Using an item that has no license data attached (a spawned or duplicated item) shows a
  _"No license data found on this item"_ notification and does nothing.

{% hint style="warning" %}
Physical licenses require an inventory that supports **item metadata** (`ox_inventory`,
`qs-inventory`, `tgiann-inventory`, `codem-inventory`, `core_inventory`, `ak47_inventory`). On an
inventory without metadata support the item cannot carry the license data, and using it will report
that no license was found. Keep `Config.UsePhysicalLicenses = false` on those setups.
{% endhint %}

***

## <mark style="color:yellow;">**Where to go next**</mark>

{% content-ref url="configuration/sh.main.lua.md" %}
[sh.main.lua](configuration/sh.main.lua.md)
{% endcontent-ref %}

{% content-ref url="script-flow.md" %}
[script-flow.md](script-flow.md)
{% endcontent-ref %}
