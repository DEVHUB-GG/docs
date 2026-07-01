---
description: >-
  How to register a new prop model in the Grid System and set up its bounding
  dimensions, offsets, and stacking behavior.
---

# 📦 Adding Props

Only models listed in the prop catalog can be placed on a grid. The catalog lives in
`configs/c.config.lua` as two tables:

* `gridProps` — used for **ground** zones.
* `gridPropsVehicle` — optional **vehicle-zone** overrides for the same models (see
  [Ground vs vehicle](#ground-vs-vehicle-sizing) below).

Each entry keys a prop model to its **footprint** — the bounding box the system uses to place it,
stack other props on it, and draw its outline in the builder.

***

## <mark style="color:yellow;">**Adding a model**</mark>

Add a new key to `gridProps` with at least a `size`:

```lua
gridProps = {
    -- ...existing props...

    ['ex_prop_adv_case_sm'] = {
        dontAllowStacking = false,
        size = { x = 0.75, y = 0.70, z = 0.47 },
    },
}
```

An entry using every field:

```lua
['ex_office_swag_ivory2'] = {
    dontAllowStacking = true,
    size         = { x = 0.74, y = 0.67, z = 0.30 },
    centerOffset = { x = 0.05, y = -0.02, z = 0.0 },
    zOffset      = -0.08,
},
```

After editing the file, restart the resource. You can now place the model with
`/spawnpropgrid <model> <uid>`.

{% hint style="info" %}
The model must be a valid prop in your game/streamed assets. Only the footprint is configured here —
the Grid System does not stream the model itself.
{% endhint %}

***

## <mark style="color:yellow;">**size**</mark>

```lua
size = { x = 0.75, y = 0.70, z = 0.47 }
```

* **Description**: The prop's bounding box in **meters**. `x` is its width, `y` its depth/length,
  and `z` its height. This box is what the grid uses for collision, stacking, and the outline drawn
  around the prop in the builder. Getting it close to the model's real dimensions gives the tightest,
  most natural packing.
* **Example**: A small case roughly 75&nbsp;cm wide, 70&nbsp;cm deep, 47&nbsp;cm tall →
  `{ x = 0.75, y = 0.70, z = 0.47 }`.

***

## <mark style="color:yellow;">**centerOffset**</mark>

```lua
centerOffset = { x = 0.0, y = 0.0, z = 0.0 }
```

* **Description**: Optional. Shifts the **center of the bounding box** relative to the model's pivot,
  in meters. Use it when a model's origin is not at its visual center, so the box lines up with what
  you actually see. Omit it (or leave all zeros) when the model is already centered.
* **Example**: `{ x = 0.05, y = -0.02, z = 0.0 }` nudges the box 5&nbsp;cm along X and 2&nbsp;cm back
  along Y to match an off-center model.

***

## <mark style="color:yellow;">**zOffset**</mark>

```lua
zOffset = 0.0
```

* **Description**: Optional. A vertical nudge (meters) applied to the prop when it spawns, without
  changing its footprint. Use it to seat a prop that otherwise floats above or sinks into the
  surface. Positive raises, negative lowers.
* **Example**: `zOffset = -0.08` drops the prop 8&nbsp;cm so it rests flush on the floor.

***

## <mark style="color:yellow;">**dontAllowStacking**</mark>

```lua
dontAllowStacking = false
```

* **Description**: Optional (defaults to `false`). When `true`, nothing can be placed on top of this
  prop — the space above it is blocked. Use it for rounded, soft, or irregular props that can't
  reasonably support another item (bags, body bags, drug packs, etc.).
* **Example**: `dontAllowStacking = true` on a duffel bag so cases can't be stacked on it.

***

## <mark style="color:yellow;">**Ground vs vehicle sizing**</mark>

The same model can behave differently in a vehicle zone (the ride's tilt and floor often need a
slightly different fit). For vehicle zones the system uses the `gridPropsVehicle` entry for a model
if one exists, and otherwise falls back to the `gridProps` entry. Add a `gridPropsVehicle` entry only
when you want a vehicle-specific footprint, offset, or `zOffset`:

```lua
gridPropsVehicle = {
    ['ex_prop_adv_case_sm'] = {
        dontAllowStacking = true,
        size    = { x = 0.75, y = 0.70, z = 0.47 },
        zOffset = 0.13,   -- seat it on the truck bed
    },
}
```

***

## <mark style="color:yellow;">**Tuning the dimensions**</mark>

The quickest way to get a prop's footprint right:

{% stepper %}
{% step %}
### Add the entry with an estimate

Give `size` your best guess and restart the resource.
{% endstep %}

{% step %}
### Place it in the builder

Run `/spawnpropgrid <model> <uid>` and press `G` to show the grid. The builder draws the bounding
box around the prop so you can see exactly how the footprint sits.
{% endstep %}

{% step %}
### Adjust and reload

Tweak `size` until the box hugs the model, use `centerOffset` to recenter the box, and `zOffset` to
seat it on the surface. Restart and place again to check.
{% endstep %}
{% endstepper %}

***

## <mark style="color:yellow;">**Registering a model at runtime**</mark>

You can also add catalog entries from another resource without editing the config, using the
`AddGridProp` / `AddGridPropVehicle` exports. They take the same field shape shown above.

{% content-ref url="exports.md" %}
[exports.md](exports.md)
{% endcontent-ref %}
