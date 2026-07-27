---
description: The panel the gym owner and their staff use, page by page.
---

# The Business Panel

Opened from a **Boss Menu** interaction point in the gym, or from a **Business Management** interaction point placed by an admin. What a given employee sees depends on the permissions of their grade: a page they cannot use is simply not offered.

The sidebar groups eleven pages into four sections.

***

## <mark style="color:yellow;">**OVERVIEW**</mark>

### Dashboard

The landing page. Revenue today, money today, active employees, company funds, the company level with its XP bar, and an alerts box that flags things like low stock or nobody on shift.

***

## <mark style="color:yellow;">**COMPANY MANAGEMENT**</mark>

### Company

The company's identity and its tax status.

| Action | What it does |
| --- | --- |
| **Change Logo** | Takes an image URL. It shows up in the panel header and on the business list. |
| **Change Company Name** | Renames the business. Can be disabled server-wide by an admin. |
| **Transfer Ownership** | Asks for a player's server ID and a price, then sends them an offer. The new owner pays, the old one is paid. Can be disabled server-wide. |
| **Company Color** | The accent color of this business, used on its badge. |
| **Pay Tax** | The tax box shows the rate, whether tax is currently **PAID** or **UNPAID**, and how much is owed. Tax is charged on income, and deposits are excluded so an owner cannot be taxed for topping up the company account. |

The **Company Level** box shows the current level and how much XP is still needed for the next one. XP comes from income at a configurable rate.

### Employees

The staff list, with a row per employee: their name, their role, their hourly wage, minutes worked, reps helped and the bonus that adds up to.

{% stepper %}
{% step %}
### Hiring

Click **Hire Player +**, enter the player's server ID, pick the grade and confirm. The target player gets an invite and has to accept it, so nobody is hired by mistake.
{% endstep %}

{% step %}
### Editing

The pen button on a row changes an employee's grade.
{% endstep %}

{% step %}
### Paying

**Pay Bonuses** banks every employee's bonus at once, calculated as hourly wage multiplied by minutes worked, divided by 60. The money leaves the company funds, and each employee then claims their share at a **Claim Bonus** point in the world.

**Pay Extra** on a single row hands one employee an extra amount on top.
{% endstep %}

{% step %}
### Resetting counters

**Reset Minutes** zeroes one employee's worked time after they have been paid. **Reset Reps Helped** does the same for the spotting counter, on one employee or on everyone at once.
{% endstep %}

{% step %}
### Firing

The fire button on a row, with a confirmation.
{% endstep %}
{% endstepper %}

### Permissions

Grades, wages and what each grade is allowed to do.

On the left, the grade list, each with its label, color and hourly wage. On the right, the eight permission switches for the selected grade:

`Manage Company`, `Manage Employees`, `Manage Permissions`, `Manage Warehouse`, `Manage Finances`, `Manage Sellers`, `Pay Bonuses`, `Transfer Ownership`.

* **Add Grade +** creates a new one. The number of grades a business may have is capped by its level, and the interface says so when you hit the ceiling.
* **Edit** renames a grade and changes its color, with a live preview of the badge.
* **Wage** sets the hourly rate that drives the bonus calculation.
* **Reset** clears every permission on that grade, **Remove Grade** deletes it.

{% hint style="info" %}
The **Owner** grade always has every permission and cannot be edited. That is deliberate: there is no way to lock yourself out of your own business.
{% endhint %}

### Progression

The company's level ladder. Each level lists what it unlocks (more grade slots, more employee slots, more warehouse slots, faster delivery, warehouse discounts, an XP bonus, or an offline worker becoming hireable), and levels the business has not reached yet are marked **Locked!**.

Underneath are the **Starter Missions** (one-off goals for a new business) and the **Weekly Missions** (a rotating set), each paying out XP on completion.

***

## <mark style="color:yellow;">**OPERATIONS**</mark>

### Warehouse

The stockroom. Every item the business is allowed to trade in, with its stock count and its price.

| Action | What it does |
| --- | --- |
| **Enabled / Disabled** toggle | Whether the item is offered in the public shop. The number of items a business may have enabled at once is capped by its level. |
| **Order +** | Buys more stock from the supplier. The money comes out of company funds and the delivery takes a while. |
| **Order Items** | A bulk order: the same quantity of every enabled item in one go. |
| **Withdraw** | Moves stock out of the warehouse into your own inventory. |
| **Deposit** | Moves items from your inventory into the warehouse. |

Items above the business's level show as **Locked**.

### Ordered Items

Deliveries that are on their way. Each order shows what it is, how much, and a countdown. Once the countdown hits zero the order is marked **READY** and a **Collect** button appears, which puts the goods into the warehouse. **Claim All** collects every ready order at once.

### Offline Workers

NPC staff that keep working while nobody is online. Four of them ship with the script:

| Worker | What it does |
| --- | --- |
| **Income Advisor** | Optimizes revenue. |
| **Sales Expert** | Increases warehouse sales volume. |
| **Auto Stocker** | Orders items automatically when stock hits zero. Higher levels check more often. |
| **Auto Collector** | Collects ready orders every 15 minutes. Higher levels collect more. |

Each has a hire cost and a daily cost that is charged automatically every 24 hours. **Upgrade** raises the worker's level and their effect, and their daily cost with it. **Stop renewal** lets the current 24 hour period run out and then dismisses them, **Resume renewal** puts them back on the payroll, and **Fire** dismisses them immediately with no refund (upgrades are kept for the next hire).

Workers unlock at business levels set by an admin on the Progression config.

***

## <mark style="color:yellow;">**FINANCIALS**</mark>

### Finances

The company account. A running **Total Company Balance**, a **Deposit** and **Withdraw** pair of quick actions, an **Income By Source** breakdown, and the full **Transaction History** with date, amount, player and description, searchable and sortable.

### Memberships

Where the owner sets the gym pass, once the zone belongs to the business.

* **Membership Required**: when on, players need an active pass to train at any zone belonging to this business.
* **Membership Packages**: add as many tiers as you like, each with a **Duration (days)**, a **Price ($)** and a comma-separated list of features shown on the buy card.
* **Daily Reward**: a free item members can claim once every 24 hours. It has to be picked from the business **warehouse**, and every claim takes one off the stock, so a reward the gym has run out of cannot be claimed.

Nothing on this page applies until **Save Changes** is pressed, and saving needs the Manage Finances permission.

### Shop

The public storefront, the same one customers see at a **Shop** interaction point. Products are the warehouse items you enabled, with their stock and sell price. Supplement products carry a tooltip explaining what the boost does and how long it lasts.

Customers add items to a cart and check out, or use **Buy Now** for an instant single purchase. Sorting covers name, price and stock in both directions.

***

## <mark style="color:yellow;">**Points in the world**</mark>

Some business features are not in the panel at all: they are interaction points an admin places, each carrying one or more modules.

| Module | What a player does with it |
| --- | --- |
| **Shop** | Opens the public storefront. |
| **Stash** | Opens a shared stash, via the `OpenStash` hook. |
| **Boss Stash** | The same, restricted to management. |
| **Business Management** | Opens this panel. |
| **Claim Bonus** | Where an employee collects the bonus that was banked for them. |
| **Call Worker** | A customer rings for staff. Employees on duty get the call, and the first to accept gets a GPS waypoint to the customer. |

{% content-ref url="admin-panel.md" %}
[admin-panel.md](admin-panel.md)
{% endcontent-ref %}
