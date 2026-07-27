---
description: >-
  The places players click to open the gym menu, the cloakroom, the locker or
  the boss menu.
---

# Interaction Points tab

Machines are not the only thing to click in a gym. An **interaction point** is a spot that carries one or more features. A gym needs at least one of them: the **Gym UI** point, which is what opens the gym menu.

The counter at the top says how many points the zone has, and the **Add Point** button starts a new one.

***

## <mark style="color:yellow;">**The four features**</mark>

| Feature | What happens when a player uses it |
| --- | --- |
| **Gym UI** | Opens the gym menu for this zone: dashboard, exercises, leaderboard, achievements, membership. |
| **Locker** | A reserved slot for your own locker behavior. It does nothing on its own. |
| **Cloakroom** | Fires an event name you type in, on the client or on the server. This is how you hook up any clothing or outfit resource. |
| **Boss Menu** | Opens the business panel. This option only appears when the zone is assigned to a business. |

One point can carry several features at once. A single reception desk can be Gym UI plus Cloakroom, and the player then gets two options on the same prop.

***

## <mark style="color:yellow;">**Adding a point**</mark>

{% stepper %}
{% step %}
### Walk to where the point should be
{% endstep %}

{% step %}
### Click **Add Point**

The **New Interaction Point** card opens.
{% endstep %}

{% step %}
### Pick how it is placed

Three buttons at the top of the card:

* **Prop**: a physical prop is spawned and the target sits on it. Best for a reception desk, a screen or a sign.
* **Coords**: no prop, just an invisible spot in the world. Best when the MLO already has furniture you want to use.
* **Ped**: an NPC is spawned and the target sits on them. Best for a receptionist.
{% endstep %}

{% step %}
### Tick the features

The **Features** grid. Tick at least one, or the point cannot be saved.
{% endstep %}

{% step %}
### Fill in the mode's own fields

{% tabs %}
{% tab title="Prop" %}
Type a **Prop Model** (for example `devhub_gym2_screen`) and click **Place with Gizmo**. The creator closes, the prop appears in front of you, position it and press `ENTER`.
{% endtab %}

{% tab title="Ped" %}
Type a **Ped Model** (for example `a_m_y_business_01`) and click **Place with Gizmo**. Position the ped and press `ENTER`. The ped keeps the heading you gave it.
{% endtab %}

{% tab title="Coords" %}
Either type **X / Y / Z** by hand, or click **Use My Position** to fill them from where you are standing. Then click **Confirm**.
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
### If you ticked Cloakroom, fill in the event

A yellow **Cloakroom Event** box appears:

* **Event Name**: the event to fire, for example `myresource:openCloakroom`.
* **Trigger Side**: **Client** fires it on the player who used the point, **Server** fires it on the server with that player as the source.
{% endstep %}

{% step %}
### Press **Save**

Interaction points live in memory until the zone is saved.
{% endstep %}
{% endstepper %}

***

## <mark style="color:yellow;">**The point list**</mark>

Each point shows `Point #1`, the mode it uses as a small badge, its coordinates and the features it carries. Use the **pen** button to reopen it and change anything, including re-placing it with the gizmo, and the **trash** button to delete it.

***

## <mark style="color:yellow;">**Wiring the cloakroom to your clothing resource**</mark>

The script does not ship a clothing menu, on purpose: every server runs a different one. The Cloakroom feature just fires the event you name, so hooking it up is a one-liner in your own resource.

```lua
-- Client side, with Trigger Side set to Client
RegisterNetEvent('myresource:openCloakroom', function()
    exports['illenium-appearance']:openOutfitMenu()
end)
```

{% hint style="info" %}
The **Locker** feature is a placeholder that fires nothing. If you want a stash at the gym, use a Cloakroom point pointed at your own stash event, or use the business module's **Stash** interaction point, which has a stash id built in.
{% endhint %}
