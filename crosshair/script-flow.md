---
description: >-
  A start-to-finish overview of what Crosshair Maker does and how to use each part
  of it, written for someone who has never touched the script before.
---

# 🌊 Script Flow

## <mark style="color:yellow;">**What is this?**</mark>

Every player builds their own crosshair. They pick a shape, colors, an outline and a center dot, and they can go further: a different reticle per weapon class, a sniper scope with mouse-wheel zoom, hit flashes, a low-ammo warning, or a crosshair drawn freehand. Designs are saved as profiles and traded with short share codes.

You stay in control from an admin panel inside the game. You decide which features exist at all, which shapes and scopes players may pick, how far every slider may go, and which keys open what. You press Save and it applies to everyone, live.

{% hint style="info" %}
The custom crosshair ships **off**. A new player keeps GTA's own reticle until they switch ours on. That is deliberate: nobody has their aim changed without asking for it.
{% endhint %}

***

## <mark style="color:yellow;">**Turning it on**</mark>

{% stepper %}
{% step %}
### Press the toggle key

**F9** by default. It turns the custom crosshair on and off at any time. Players can rebind it themselves in **GTA Settings → Key Bindings → FiveM**.
{% endstep %}

{% step %}
### Open the maker

Type **`/crosshair`**. You can rename that command, switch it off, or give players a key that opens the maker instead. It is all in the panel, under **General**.
{% endstep %}

{% step %}
### Build a crosshair

The maker has a live preview: every slider you touch is drawn immediately, so nobody is editing blind.
{% endstep %}
{% endstepper %}

***

## <mark style="color:yellow;">**Style: the shape, the colors and the dot**</mark>

One tab holds the whole look of the crosshair.

* **Shape**: Cross, T-Cross, X-Cross, Dot, Circle, Circle + Cross, Chevron or Brackets. You choose in the panel which of the eight players may pick.
* **Geometry**: line length, thickness, gap, sharpness (100 is razor sharp, lower gives a soft glow) and rounded ends. Circle shapes get a radius and a ring thickness.
* **Colors**: the main color and the overall opacity.
* **Outline**: a contour around every shape, with its own color, thickness and opacity, so the crosshair stays readable against a white wall and a night sky alike.
* **Center dot**: size, opacity, and either the main color or a color of its own. With the **Dot** shape, the dot is the whole crosshair.
* **Dynamic gap**: the crosshair opens up while shooting or moving, like in a shooter. Off by default, with separate amounts for firing and for moving.

The house crosshair is called **DevHub Gold**: a rounded gold cross with a center dot. It is what a new player starts from, and what **Reset** goes back to.

***

## <mark style="color:yellow;">**A different crosshair per weapon**</mark>

Under **Per Weapon**, a player assigns a **saved profile** to each weapon class, so a pistol and a sniper never share a reticle.

{% stepper %}
{% step %}
### Save some profiles first

Build a crosshair, go to **Profiles**, name it and save. Per-weapon rules point at profiles, so there is nothing to assign until at least one exists.
{% endstep %}

{% step %}
### Switch per-weapon crosshairs on

Then set each class: Pistols, SMGs, Shotguns, Rifles, Snipers, Machine Guns, Heavy, Melee & Fists, Throwables and Everything Else.
{% endstep %}

{% step %}
### Pick a profile, the main crosshair, or nothing

A class left on **Main crosshair** uses the player's main design. Set it to **Hidden** and that class shows no crosshair at all. The tab marks the class the player is holding right now, so they can see the change as they swap guns.
{% endstep %}
{% endstepper %}

A weapon follows its GTA weapon class automatically. If you want one specific gun to break that rule, do it in the panel under **Weapons**.

***

## <mark style="color:yellow;">**The scope**</mark>

A sniper-style overlay that replaces plain aiming.

* **Aim to scope in, scroll the mouse wheel to zoom.**
* Ten reticles: Classic, Thin, Mil-Dot, Delta, Duplex, Mil-Hash, Holdover Tree, Rangefinder, Minimal and Dotted.
* Set the reticle's color, opacity and thickness, its center dot, the number of rings, and how much of the screen edge is blacked out.
* Zoom is a range: a minimum, a maximum, a step, and the zoom you start at. It can show a zoom indicator and remember the last zoom between aims.
* **Hold breath**: hold **Shift** while scoped to steady the aim, with an air bar showing how long it lasts.

Players choose whether their scope applies to **snipers only** or to **all guns**. You have the last word in the panel, under **Scope rules**: which weapon groups may scope at all, which count as sniper rifles, whether snipers keep GTA's own scope view, whether scoping works inside a vehicle, and a hard **max zoom** cap no player can go past.

***

## <mark style="color:yellow;">**Feedback: hits, ammo and threats**</mark>

Three small assists, each with a "Try it" button in the maker so players can see them without firing a shot.

* **Hit confirm**: the reticle flashes an X when a shot lands. A takedown flashes bigger, in its own color.
* **Ammo alert**: a magazine chip pulses under the crosshair while aiming with a nearly empty clip. The threshold is a percentage of the magazine.
* **Threat tint**: the crosshair changes color while it is on a living character.

Any of the three can be switched off server-wide in the panel, and then it disappears from the maker entirely.

***

## <mark style="color:yellow;">**Drawing your own crosshair**</mark>

The **Sketchpad** lets a player draw a reticle by hand: a pen, a rubber, a brush size and an undo. Symmetry does the tedious part: draw one arm with **Fourway** on and the other three are mirrored as you go.

{% hint style="warning" %}
A hand-drawn crosshair is too detailed to fit in a share code. Save it as a profile instead.
{% endhint %}

***

## <mark style="color:yellow;">**Profiles, presets and share codes**</mark>

* **Presets** are one-click starting points: **DevHub Gold**, Classic Green, Red Dot, Cyan Ring, Pro X, Violet Chevron, Marksman and Tactical Brackets. Applying one overwrites the crosshair the player is using, after a confirmation, and never touches their saved profiles.
* **Profiles** are named crosshairs a player saves, loads and deletes. Twelve per player by default, which you can change in the panel.
* **Share codes** are short codes in the style of CS2. A player exports their crosshair as a code, sends it to a friend, and the friend pastes it in to get the same reticle. Every saved profile can be copied as a code too.

***

## <mark style="color:yellow;">**What the admin panel controls**</mark>

Open it with **`/crosshairadmin`**. It also appears as **Crosshair** in devhub\_lib's `/admindevhub` hub, which opens for devhub\_lib admins only, so an admin who got access through the ACE permission or an identifier uses the command instead.

Everything here applies to every player on the server, live. **Save & apply** writes it, **Reset to config** throws the overrides away and goes back to what the config file says. Neither one deletes a player's crosshair or their saved profiles: what changes is that a value now outside your limits, or a shape you just switched off, is pulled back into what you allow.

* **Features**: ten master switches (dynamic gap, hit confirm, ammo alert, threat tint, sketchpad, per-weapon, scope, profiles, presets, share codes). Switching one off removes it from every player's maker, and makes the matching export a no-op.
* **Crosshair types**: which of the eight shapes players may pick. At least one has to stay on.
* **Scope rules**: which weapon groups may scope, which count as snipers, which scope styles exist, the hard zoom cap, the overlay magnification, whether snipers keep GTA's own scope, scoping in vehicles, and hold-breath timings.
* **Limits**: the minimum and maximum of every slider in the maker. Narrow them and existing player crosshairs are pulled back into range automatically, rather than drifting until the next restart.
* **General**: the player command and whether it exists at all, the toggle key, an optional maker key, whether GTA's own reticle is hidden, how many profiles a player gets, and debug mode.
* **Weapons**: per-gun exceptions. Give one gun a different class, or let it use the scope like a sniper rifle.

{% hint style="warning" %}
Switching a command or a key **off** takes effect immediately. **Renaming** a command, or changing a default key, reaches a player when they reconnect. FiveM cannot un-register a command mid-session.
{% endhint %}

***

## <mark style="color:yellow;">**Where to go next**</mark>

{% content-ref url="configuration/README.md" %}
[configuration](configuration/README.md)
{% endcontent-ref %}

{% content-ref url="exports-and-events.md" %}
[exports-and-events.md](exports-and-events.md)
{% endcontent-ref %}
