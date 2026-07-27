---
description: >-
  Make every prop of one model trainable across the whole map, without placing
  anything.
---

# Global Models

Placing machines zone by zone gives you exact control. **Global Models** is the opposite approach: switch a prop model on, and **every prop with that model, anywhere in Los Santos**, becomes trainable.

No zone, no membership, no placement. If the map already has a bench in a police station locker room, a global model turns it into a working bench press.

***

## <mark style="color:yellow;">**Turning a model on**</mark>

{% stepper %}
{% step %}
### Open **Global Models** in the sidebar

The list shows every exercise the script knows, grouped by category, with its prop model underneath the name.
{% endstep %}

{% step %}
### Find the one you want

Use the search box at the top. It matches both the exercise name and the prop model.
{% endstep %}

{% step %}
### Click the toggle on its row

The row lights up. The tooltip reads **Enable global** when it is off and **Disable global** when it is on.
{% endstep %}

{% step %}
### Pick the minigames (optional)

Click the blue **gamepad** button on the row. It works exactly like the panel on a placed machine: apply a saved **preset** with one click, or tick minigames by hand and set a difficulty range for each.
{% endstep %}

{% step %}
### Click **Save** in the top right

There is no per-row save. One button saves the whole page.
{% endstep %}
{% endstepper %}

***

## <mark style="color:yellow;">**What this is good for**</mark>

* Making the vanilla gym props around the map usable without building a zone at every one of them.
* A quick server-wide "you can train anywhere" ruleset.
* Prototyping. Switch a model on, try it in game, switch it back off.

***

## <mark style="color:yellow;">**What to watch out for**</mark>

{% hint style="warning" %}
Global models ignore memberships and ignore zones. A player can train on them for free, wherever they stand. If your economy relies on paid gym passes, keep the models you sell access to out of this list.
{% endhint %}

{% hint style="info" %}
A prop that is inside one of your zones **and** enabled globally will still work, but the zone's copy is the one that carries the zone's rules. Pick one approach per model and stick to it.
{% endhint %}

{% hint style="info" %}
Detection range and how often the script scans for these props are in `configs/client.lua` under `Config.GlobalModels`. Raising the range or lowering the interval costs performance on every client.
{% endhint %}
