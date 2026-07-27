---
description: Creating, opening and deleting gym zones.
---

# All Zones

A **zone** is one gym. It owns its machines, the props it hides, its interaction points, its membership packages, its ped-clear areas and its map blip. A server can run as many zones as you like, and each one is independent: different prices, different equipment, different rules.

***

## <mark style="color:yellow;">**The zone list**</mark>

The first page the creator opens on. Every zone is a card:

* **Name** and **ID** at the top left. The ID is the database row and never changes.
* Three counters: **Equipment**, **Removed** and **Points**.
* A badge at the bottom: **Membership Required** (green) or **Free Access** (gray).
* A red trash button at the top right.

Click anywhere on a card to open it in the Zone Editor.

***

## <mark style="color:yellow;">**Creating a zone**</mark>

{% stepper %}
{% step %}
### Click **Create Zone**

Top right of the page. On an empty server the same button sits in the middle of the screen as **Create your first zone**.
{% endstep %}

{% step %}
### The zone is created immediately

It is called `New Zone` and the editor opens on it. The row already exists in the database, so there is nothing to confirm.
{% endstep %}

{% step %}
### Rename it

Go to the **Settings** tab and type a real name in **Zone Name**, then press **Save**.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
A zone has no coordinates of its own. Where the gym *is* comes from the machines and the interaction points you place inside it. That is why you should stand in the right building before you start.
{% endhint %}

***

## <mark style="color:yellow;">**Deleting a zone**</mark>

{% stepper %}
{% step %}
### Click the red trash icon on the card
{% endstep %}

{% step %}
### Read the confirmation

It tells you exactly what goes with it: the zone, its equipment, its removed props and its interaction points, with the counts filled in.
{% endstep %}

{% step %}
### Click **Delete**
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
There is no undo and no recycle bin. If the zone is assigned to a business, unassign it first from the business admin panel unless you also want the business to lose its gym.
{% endhint %}

***

## <mark style="color:yellow;">**Zones and businesses**</mark>

A zone can stand on its own or belong to a business. Standalone is the default and needs nothing extra: memberships and the daily reward are configured on the zone's own **Settings** tab.

Once a zone is assigned to a business (from the business admin panel, not from here), three things change:

* Membership packages and the daily reward are managed from the **business panel** instead, by the owner.
* The **Boss Menu** feature becomes available on interaction points.
* Employees of that business can spot players who are mid-set.

{% content-ref url="../businesses/admin-panel.md" %}
[admin-panel.md](../businesses/admin-panel.md)
{% endcontent-ref %}
