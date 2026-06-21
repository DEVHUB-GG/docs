---
description: >-
  A start-to-finish overview of what the License System does and how to use each
  part of it — written for someone using the script for the first time.
---

# 🌊 Script Flow

## <mark style="color:yellow;">**What is this?**</mark>

The License System lets your server issue, carry, verify, and manage in-game licenses and IDs.
Admins design license templates in a visual creator; players pick licenses up from a city clerk,
carry them in a card holder, show them to others, and get them checked. There is also an optional
underground "fake ID" path with crafting stations and a delivery mission.

***

## <mark style="color:yellow;">**Opening the interfaces**</mark>

Every interface can be opened by a chat command, a usable inventory item, an event, or an optional
keybind — configured in [`sh.main.lua`](configuration/sh.main.lua.md) under `Config.Usage`. By default:

* **License MDT** — `/licenseMdt` or the `devhub_licensesmdt` item. The management tablet (dashboard,
  lookup, profiles, and the license creator for admins).
* **Card Holder** — `/licenses` or the `devhub_licenseholder` item. Shows the licenses you carry.
* **Favorites Hand** — `/favorites` or the `devhub_favlicenses` item. Shows your favorite licenses
  in hand to display them quickly.

{% hint style="info" %}
With `Config.UsePhysicalLicenses = true`, licenses are physical inventory items and the `/licenses`
and `/favorites` commands are disabled — players use the items instead.
{% endhint %}

***

## <mark style="color:yellow;">**Getting a license**</mark>

{% stepper %}
{% step %}
### Visit the License Pickup NPC

Head to the city clerk (the License Pickup blip on the map). Their location is set by
`Config.LicensePickUp`.
{% endstep %}

{% step %}
### Choose a license and pay

Pick the license you want. If it has a price, choose your payment method (cash or card, per the
template's settings). Some licenses may be free or auto-issued on first join.
{% endstep %}

{% step %}
### Receive and carry it

The license is added to your card holder. Lost, expired, or revoked licenses can be picked up again
(controlled by `Config.RetakeableLicenseStatus`).
{% endstep %}
{% endstepper %}

You can also buy holder/MDT/favorites items from the same NPC (`Config.ItemShop`).

***

## <mark style="color:yellow;">**Showing & checking licenses**</mark>

* **Show your license** — from the card holder, choose **Show**. Nearby players within
  `Config.LicenseShowDistance` see it on screen. Use the on-screen controls to focus, edit its
  display position, or close it. A cooldown (`Config.LicenseShowCooldown`) prevents spam.
* **Check someone else** — target another player and choose **Check Licenses** (enabled via
  `Config.PlayerTarget`). A search animation plays and you view their card holder. If
  `allowTakeLicense` is on, you can take a license from them; players can also **Give** licenses to
  nearby players.

***

## <mark style="color:yellow;">**Managing licenses (MDT)**</mark>

Open the **License MDT** to:

* View the **dashboard** stats and recent activity.
* **Look up** a license by ID or owner, and **search profiles**.
* **Grant or revoke** license permissions (subject to job/grade rules on the template).
* **Change a license status** (active, suspended, lost, expired, revoked) with a reason.

The built-in **License Creator** (opened from the MDT) is where templates are designed — layout,
background, texture, components, icons, data fields, pricing, expiration, and item integration. By
default it is admin-only (`Config.CreatorOnlyForAdmins`).

***

## <mark style="color:yellow;">**The fake ID path (underground)**</mark>

{% stepper %}
{% step %}
### Find the hideout

Enter the underground hideout (`Config.HideoutLocations`) and speak to **John**, the underground
contact (`Config.UndergroundNpc`).
{% endstep %}

{% step %}
### Buy supplies and take an order

Buy the fake-license supplies John sells (`Config.UndergroundItemShop`), and optionally take a
fake ID delivery order.
{% endstep %}

{% step %}
### Craft the fake license

Use the crafting stations in the hideout — the **Paper Cutter**, **Foil Tipper**, and **Embosser**
(`Config.Papercutter`, `Config.Foiltipper`, `Config.Embosser`) — to forge a fake version of an
eligible license. Pick an avatar photo from `Config.FakeIdPhotos`.
{% endstep %}

{% step %}
### Deliver it

If you took an order, deliver the finished fake to the buyer at the marked drop-off
(`Config.FakeIdMissionConfig`) to get paid. Use `/cancelMission` to abandon an active order.
{% endstep %}
{% endstepper %}

***

## <mark style="color:yellow;">**Scanning at a terminal**</mark>

Licenses can be verified at a scan terminal. Players drag and hold a license to scan it; the result
(verified / failed) is shown. Other resources can open a terminal with a custom scan callback via
the exports — see the Exports page.

***

## <mark style="color:yellow;">**Where to go next**</mark>

{% content-ref url="configuration/README.md" %}
[configuration](configuration/README.md)
{% endcontent-ref %}

{% content-ref url="exports-and-events.md" %}
[exports-and-events](exports-and-events.md)
{% endcontent-ref %}
