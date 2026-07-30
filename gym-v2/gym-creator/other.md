---
description: Clearing ambient NPCs out of the gym.
---

# Other tab

One setting lives here, and it solves the single most annoying thing about a gym MLO: pedestrians wandering through the middle of your bench press.

***

## <mark style="color:yellow;">**Ped clearing**</mark>

Switch it on and the script continuously removes ambient NPCs inside the areas you define. You can add as many areas as you need, so an L-shaped building can be covered by two or three overlapping circles.

{% stepper %}
{% step %}
### Open the **Other** tab and switch **PED CLEARING** on

The area list appears underneath.
{% endstep %}

{% step %}
### Click **Add Area**

A new **Area #1** card is added, starting at your position.
{% endstep %}

{% step %}
### Set the center

Type **X / Y / Z** by hand, or click the blue **person** button to snap the area to where you are standing.
{% endstep %}

{% step %}
### Set the radius

The slider runs from 1 to 200 meters. The current value is shown in meters next to the label and in the card header.
{% endstep %}

{% step %}
### Check it with the eye button

Click the **eye** and the area is drawn in the world so you can see exactly what it covers. Press **RIGHT SHIFT** to peek out of the interface while it is drawn. Click the button again (it now shows a stop icon) to hide it.

The preview only runs while you are on the **Other** tab. Leaving the tab turns it off by itself.
{% endstep %}

{% step %}
### Press **Save**
{% endstep %}
{% endstepper %}

Use the **trash** button on a card to delete that area.

***

## <mark style="color:yellow;">**How it behaves in game**</mark>

* NPCs are cleared on a loop, so ones that walk in later are removed too.
* Clearing only runs while a player is near the area, so an empty gym on the other side of the map costs nothing.
* Players are never touched, only ambient pedestrians.

{% hint style="info" %}
The loop interval and how close a player has to be before clearing starts are in `configs/client.lua` under `Config.PedClear`. The defaults are fine for almost every server.
{% endhint %}

{% content-ref url="../configuration/client.lua.md" %}
[client.lua.md](../configuration/client.lua.md)
{% endcontent-ref %}
