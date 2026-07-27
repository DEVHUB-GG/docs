---
description: >-
  The optional module that turns a gym into a player-owned company with staff,
  a warehouse, a shop and a balance sheet.
---

# 💼 Businesses

Gym V2 can run in two modes.

{% tabs %}
{% tab title="Standalone (default)" %}
The gym belongs to nobody. You configure its membership packages and its daily reward on the zone's own [settings.md](../gym-creator/settings.md "mention") tab, players train, and that is the whole system.

Nothing extra to install, nothing extra to enable.
{% endtab %}

{% tab title="As a business" %}
The gym belongs to a player. They hire staff, pay wages, run a warehouse, sell supplements from a shop, pay tax, level the company up and unlock modules as they go. Their employees can spot players mid-set for a bonus.

This is a separate DevHub license and is **off by default**.
{% endtab %}
{% endtabs %}

***

## <mark style="color:yellow;">**Turning it on**</mark>

The business module is unlocked by your license. If you own it, the entry appears by itself: `/admindevhub`, **Gym**, and a **Business** button sits above **Gym Creator**.

If you only see **Gym Creator**, the module is not licensed on this server and everything on the following two pages is inactive. The gym works fully without it.

***

## <mark style="color:yellow;">**How a gym becomes a business**</mark>

{% stepper %}
{% step %}
### Build the gym first

Create the zone, place the equipment and the interaction points. See [gym-creator](../gym-creator/ "mention").
{% endstep %}

{% step %}
### Create the business

`/admindevhub`, **Gym**, **Business**, then **All Businesses** and **Create Business**. You give it a name and the owner's server ID.
{% endstep %}

{% step %}
### Assign the zone to it

Open the business, go to its **Zones** tab, find your gym in the list and click **Assign to Business**.
{% endstep %}

{% step %}
### Give the owner a way in

Back in the Gym Creator, add an interaction point with the **Boss Menu** feature. That option only appears once the zone is assigned, which is why this step comes last.
{% endstep %}
{% endstepper %}

{% content-ref url="admin-panel.md" %}
[admin-panel.md](admin-panel.md)
{% endcontent-ref %}

{% content-ref url="business-panel.md" %}
[business-panel.md](business-panel.md)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**What changes once a zone belongs to a business**</mark>

| Before | After |
| --- | --- |
| Membership packages set on the zone's Settings tab. | Set by the owner on the business panel's **Memberships** page. |
| Daily reward is a free-typed item name. | Has to be an item stocked in the business warehouse. |
| Supplements handed out however you like. | Ordered into the warehouse and sold from the business shop. |
| Membership income goes nowhere. | Goes into the company funds, minus tax. |
| Nobody can spot a player mid-set. | Employees can, and earn a bonus for it. |
