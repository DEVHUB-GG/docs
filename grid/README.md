---
description: >-
  Developer reference for the DevHub Grid System — how to lay out props on a
  zone grid with the in-game builder and place them from a script via exports.
---

# 🔲 GRID SYSTEM

{% hint style="info" %}
This is a **developer / integration reference**. It covers the builder commands and the exports
you use to place props from your own resources. The in-game builder commands are a dev-time tool
and are **disabled by default** — see [Builder Commands](commands.md).
{% endhint %}

The Grid System places props on an invisible, snapping grid inside a **zone** you define. You use it
in two phases:

1. **Build time** — lay a scene out by hand with the in-game editor and capture every prop's
   position.
2. **Runtime** — replay that captured layout from a script with the placement exports, with no
   editor and no UI.

## Core concepts

* **Zone** — a rectangular area defined by four corner coordinates plus a maximum stacking height.
  A zone can sit on the **ground** or be attached to a **vehicle** so its props move with it. Every
  prop position is stored **relative to its zone**.
* **Zone-local coordinates** — prop positions are given in meters relative to the zone, not world
  coordinates:
  * `x` — distance along the edge from corner&nbsp;1 → corner&nbsp;2
  * `y` — distance along the edge from corner&nbsp;1 → corner&nbsp;4
  * `z` — height above the zone floor
  These are exactly the values the editor reports and the placement exports expect. Because they are
  relative, a captured layout can be re-placed at any zone, anywhere in the map.
* **Prop catalog** — only models registered in `gridProps` (ground) or `gridPropsVehicle` (vehicle)
  in `configs/c.config.lua` can be placed. Each entry defines the prop's bounding size and offsets.
  Register your own models at runtime with `AddGridProp` / `AddGridPropVehicle` (see
  [Exports](exports.md)).
* **Ownership & lifetime** — a zone is owned by a player; the zone and its props live while that
  player is connected and are removed when they disconnect. Choose the owner when creating the zone
  (`RequestCreateZone(..., ownerServerId)`) or reassign it later (`SetZoneOwner`).
* **Streaming** — placed props are streamed in and out automatically based on how close players are
  to the zone, so a large scene has no cost for players who are far away.

## The build → capture → replay workflow

{% stepper %}
{% step %}
### Enable the builder tools

Set `Config.Commands = true` in `configs/sh.config.lua` (dev server only) and restart the resource.
{% endstep %}

{% step %}
### Create a zone

Run `/createzone <uid>` where you want the scene. Edit the corner coordinates in the `/createzone`
command (`configs/c.commands.lua`) to your own location first.
{% endstep %}

{% step %}
### Place each prop by hand

Run `/spawnpropgrid <model> <uid>` and position the prop with the editor controls, then press
`Enter` to place it. Repeat for every prop.
{% endstep %}

{% step %}
### Capture the layout

Run `/dumpzoneprops <uid>`. This prints a copy-pasteable Lua block of every prop's model, local
position, and rotation, with a `data = nil` slot on each line to fill in.
{% endstep %}

{% step %}
### Replay it in production

Paste the captured block into your own resource and place it with `PlaceGridProps` (no editor, no
commands needed). See [Exports](exports.md).
{% endstep %}
{% endstepper %}

***

## Pages

{% content-ref url="commands.md" %}
[commands.md](commands.md)
{% endcontent-ref %}

{% content-ref url="adding-props.md" %}
[adding-props.md](adding-props.md)
{% endcontent-ref %}

{% content-ref url="exports.md" %}
[exports.md](exports.md)
{% endcontent-ref %}
