---
description: >-
  The admin side of the business module: creating businesses, assigning gym
  zones, placing feature points and setting the server-wide rules.
---

# The Business Admin Panel

Open it with `/admindevhub`, then **Gym**, then **Business**. It only appears if the business module is licensed on your server.

Six pages in the sidebar, in two groups.

***

## <mark style="color:yellow;">**BUSINESSES**</mark>

### All Businesses

The list of every business on the server, with totals across the top (businesses, employees, funds) and a row per business showing its ID, name, level, employee count, funds and income today.

**Creating one:**

{% stepper %}
{% step %}
### Click **Create Business**
{% endstep %}

{% step %}
### Enter a **Business Name**
{% endstep %}

{% step %}
### Enter the **Owner Server ID**

The player has to be online. They become the owner with the default grade set.
{% endstep %}

{% step %}
### Click **Create**
{% endstep %}
{% endstepper %}

The trash button deletes a business. The confirmation spells out what goes with it: employees, grades, warehouse stock, pending orders and finances. There is no undo.

### Business Lookup

Open any business and inspect or fix anything in it. Nine tabs across the top:

| Tab | What you can do |
| --- | --- |
| **Overview** | Funds, level, XP progress, income today, and the tax box (rate, pending amount, when it was last paid). |
| **Grades** | Read the grade list with wages and which one is the default. |
| **Warehouse** | See stock and prices, and **Add Stock** by item name and amount when a player loses goods to a bug. |
| **Workers** | Whether each offline worker is hired and at what level. |
| **Transactions** | The full financial history. |
| **Missions** | Progress on the starter and weekly missions. |
| **Settings** | Edit the business name, logo, income tax %, XP rate, helper reps per minigame and the business level directly. Also the business coordinates and radius, with **Current Pos** to capture your position and **Preview Freecam** to fly around and check the radius. |
| **Interaction Points** | Place the business's own feature points, see below. |
| **Zones** | Assign and unassign gym zones. |

**Employees** are managed from the Overview tab: add one by server ID or by identifier, change a grade, or remove someone.

{% hint style="warning" %}
Changing the **Business Level** by hand resets that business's in-level XP to 0.
{% endhint %}

***

### Assigning a gym zone to a business

{% stepper %}
{% step %}
### Open the business, then the **Zones** tab

Every gym zone on the server is listed with its equipment and point counts, and a badge: **AVAILABLE**, **ASSIGNED** (to this business) or **Other Business**.
{% endstep %}

{% step %}
### Click **Assign to Business** on the zone you want

A zone can only belong to one business at a time.
{% endstep %}

{% step %}
### Unassigning

The same button reads **Unassign Zone** once the zone is attached. The zone and everything in it stays intact, it just goes back to standing on its own.
{% endstep %}
{% endstepper %}

***

### Placing business interaction points

These are separate from the gym zone's own points. They carry the business feature modules: Stash, Boss Stash, Shop, Business Management, Call Worker and Claim Bonus.

The editor walks you through it in three steps:

{% stepper %}
{% step %}
### Step 1: how it is placed

**Prop**, **Ped** or **Coords**.

* **Prop**: choose a model from the list or type your own, then **Place with Gizmo**. Position it and press `ENTER`.
* **Ped**: click **Place Ped (stand + ENTER)**, walk to where the ped should stand, face the direction they should face, and press `ENTER`. Then type the **Ped Model**.
* **Coords**: set the position and a radius. **Current Pos** fills it from where you stand, and **Freecam Placement** lets you fly to the spot instead.
{% endstep %}

{% step %}
### Step 2: pick the modules

Tick the features this point should carry. One point can carry several.
{% endstep %}

{% step %}
### Step 3: save

**Save Interaction Point**. Stash and Boss Stash get an auto-generated stash id at this moment, which is the id passed to your `OpenStash` hook.
{% endstep %}
{% endstepper %}

Existing points can be teleported to, edited or deleted from the same tab.

{% content-ref url="../configuration/server.lua.md" %}
[server.lua.md](../configuration/server.lua.md)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**CONFIGURATION**</mark>

These four pages are server-wide. They apply to every business at once.

### Warehouse Items

The master catalog of everything any business can stock.

Each item has a **Name** (the inventory spawn name), a **Price** (what the business pays the supplier), a **Sell Price** (the default shop price), a **Req Level** (the business level needed to unlock it), a **Max Stock**, a **Category** and an optional **Businesses** list.

Leave **Businesses** empty and every business gets the item. Fill it with business IDs and only those businesses do, which is how you give one gym an exclusive product.

**Shop Categories** at the bottom of the page defines the category tabs the storefront is grouped into, each with a key, a label and an icon.

Remember to press **Save Config**: adding or removing an item only stages the change.

### Offline Workers

The four NPC workers, with their **Hire Cost**, **Daily Cost**, **Daily/Level** surcharge, **Upgrade Cost**, **Upgrade Multiplier** and **Max Level**. Each can be switched off entirely.

The level at which a worker becomes hireable is not set here: it is a milestone on the Progression page.

### Progression

The level ladder every business climbs.

* **XP & Level Settings**: **Base XP (for Lvl 2)**, **Multiplier per Level**, **Max Level** and **Income to XP Rate (%)**. The formula is `requiredXP = BaseXP x (Multiplier ^ (level - 1))`.
* **Level Milestones**: for each level, the modules it unlocks. The available modules are Grade Slots, Employee Slots, Warehouse Slots, Faster Delivery, Warehouse Discount, XP Bonus and Worker Unlock. A business merges every milestone at or below its level, so a level 5 business has everything levels 1 through 5 grant.
* **Weekly Missions** and **Starter Missions**: each with a title, description, type, target and XP reward.

### Global Settings

Server-wide switches and economy dials.

| Setting | What it does |
| --- | --- |
| **Transfer Ownership** | Whether players may sell their business to another player. |
| **Name Change** | Whether players may rename their business. |
| **Avatar Change** | Whether players may change the business logo. |
| **Base XP / Multiplier / Max Level / Income to XP Rate** | The same level curve as on the Progression page. |
| **Price Multiplier** | The default shop sell price is the warehouse price multiplied by this. |
| **Min / Max Delivery Time** | The range, in minutes, a warehouse order takes to arrive. |

{% hint style="info" %}
Turning off Transfer Ownership, Name Change or Avatar Change hides those buttons from every business panel on the server. It is the quickest way to lock down a roleplay economy without touching permissions.
{% endhint %}
