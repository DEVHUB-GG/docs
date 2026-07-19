---
description: >-
  The in-game builder commands used to create zones, place props by hand, and
  capture a layout for scripted placement.
---

# 🎫 Builder Commands

The builder commands let you lay a scene out visually and capture it. They are a **build-time tool**.

{% hint style="danger" %}
The commands are **disabled by default**. Enable them only on a development server: set
`Config.Commands = true` in `configs/sh.config.lua` and restart the resource. Turn them back off
before going live.
{% endhint %}

Once captured, a scene is placed at runtime from your own resource with the exports — no commands
required.

{% content-ref url="exports.md" %}
[exports.md](exports.md)
{% endcontent-ref %}

***

## <mark style="color:yellow;">**/createzone**</mark>

```
/createzone <uid> [persistent]
```

* **Description**: Creates a ground zone with the given unique id. The four corner coordinates and
  the `moveStep` / `maxHeight` are read from the `gridConfig` defined inside the `/createzone`
  command in `configs/c.commands.lua` — edit those to your own location before running it. Pass
  `persistent` as the second argument to create a server-owned zone that survives your disconnect
  (see [Exports](exports.md) for the `persistent` option).
* **Example**: `/createzone warehouse1` or `/createzone warehouse1 persistent`

***

## <mark style="color:yellow;">**/createvehiclezone**</mark>

```
/createvehiclezone <uid>
```

* **Description**: Creates a zone attached to the vehicle you are currently sitting in, so its props
  move and rotate with the vehicle. You must be in a vehicle to run it.
* **Example**: `/createvehiclezone van1`

***

## <mark style="color:yellow;">**/spawnpropgrid**</mark>

```
/spawnpropgrid <model> <uid>
```

* **Description**: Enters the placement editor for `<model>` in zone `<uid>`. The model must be
  registered in `gridProps` (or `gridPropsVehicle` for vehicle zones). Position the prop with the
  controls below and press `Enter` to place it. Run it again for each prop you want to add.
* **Example**: `/spawnpropgrid ex_prop_adv_case_sm warehouse1`

**Placement editor controls**

| Key | Action |
|-----|--------|
| Arrow keys | Move the prop across the grid |
| `Page Up` / `Page Down` | Raise / lower the prop |
| `H` | Rotate the prop 90° |
| `N` | Snap next to the nearest placed prop |
| `B` | Cycle the move-step size |
| `G` | Toggle the grid overlay |
| `Enter` | Place the prop |
| `ESC` | Cancel and exit |

***

## <mark style="color:yellow;">**/removepropgrid**</mark>

```
/removepropgrid <uid>
```

* **Description**: Enters removal mode for the zone. Press `E` to cycle through the removable props,
  `Enter` to remove the highlighted one, `G` to toggle the grid, and `ESC` to exit.
* **Example**: `/removepropgrid warehouse1`

***

## <mark style="color:yellow;">**/dumpzoneprops**</mark>

```
/dumpzoneprops <uid>
```

* **Description**: Prints a copy-pasteable Lua block of every prop currently placed in the zone —
  model, zone-local position, and rotation, with a `data = nil` slot on each line to fill in. Paste
  the block into your own resource and replay it with `PlaceGridProps`.
* **Example**: `/dumpzoneprops warehouse1`

***

## <mark style="color:yellow;">**Demo commands**</mark>

`configs/c.commands.lua` also ships three commands that exercise the placement exports end to end.
They are labelled as test/demo helpers — remove them before production.

```
/testgridproduction <uid>   -- creates a 4x4m zone at your feet and drops a batch of props via the exports
/testgridplace <uid>        -- places a single prop via the export
/deletegridzone <uid>       -- deletes a zone and all its props
```

* **Description**: A quick way to verify the exports work anywhere without laying out a scene by
  hand. `<uid>` is optional and defaults to `testscene`.
