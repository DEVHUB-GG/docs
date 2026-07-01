---
description: >-
  Exports exposed by the Grid System for creating zones, placing props from a
  script, and registering prop models.
---

# 📚 Exports

The Grid System exposes **client exports** for zones and prop placement, a **server export** for
zone ownership, and two **prop-catalog helpers** for registering models at runtime. Prop positions
are always given in **zone-local meters** (see [the coordinate model](README.md)).

## <mark style="color:yellow;">**Client Exports**</mark>

Call these from any client script as `exports['devhub_grid']:<name>(...)`.

```lua
-- Zones
exports['devhub_grid']:RequestCreateZone(gridConfig, uid, debug, ownerServerId)  -- bool
exports['devhub_grid']:RequestDeleteZone(uid)                                    -- bool

-- Placing props from a script
exports['devhub_grid']:PlaceGridProp(uid, model, position, rotation, data)       -- bool
exports['devhub_grid']:PlaceGridProps(uid, propList)                             -- placed, total

-- Builder helpers
exports['devhub_grid']:DumpGridZone(uid)                                         -- table | nil
exports['devhub_grid']:ActivateEditMode(uid, mode, model, data)                  -- status, model, data
```

* **RequestCreateZone(gridConfig, uid, debug, ownerServerId)**: Creates a zone and returns `true` on
  success. `uid` is a unique id you choose. `debug` is optional (`true` opens the zone in view mode
  with the grid visible). `ownerServerId` is optional — pass a connected player's server id to make
  the zone (and its props) live for that player's session; omit it to make the calling player the
  owner. A ground `gridConfig` looks like:

  ```lua
  {
      moveStep  = 0.1,   -- grid step in meters
      maxHeight = 5.0,   -- max stacking height in meters
      coords = {         -- four corners; corner 1 is the origin, 1->2 is X, 1->4 is Y
          vec3(1764.5430, 3242.1836, 41.1580),
          vec3(1774.2539, 3244.5032, 41.1580),
          vec3(1775.9097, 3237.3862, 41.1580),
          vec3(1766.3484, 3234.9597, 41.1580),
      },
  }
  ```

* **RequestDeleteZone(uid)**: Removes the zone and all of its props for every player. Returns `true`
  on success.

* **PlaceGridProp(uid, model, position, rotation, data)**: Places a single prop in an existing zone
  and returns `true` on success.
  * `position` — a table of zone-local meters: `{ x = , y = , z = }`.
  * `rotation` — optional; accepts `0/1/2/3` (steps of 90°) or `0/90/180/270` (degrees). Defaults to `0`.
  * `data` — optional; any value you want stored on the prop and handed back later.
  * The `model` must be registered in `gridProps` / `gridPropsVehicle`. The zone must already exist;
    the export waits briefly for it, so calling `RequestCreateZone` then `PlaceGridProp` back to back
    is fine.

* **PlaceGridProps(uid, propList)**: Places many props in one call and returns `placed, total`.
  Each entry is `{ model = , position = { x, y, z }, rotation = , data = }` — the exact shape
  `DumpGridZone` / `/dumpzoneprops` prints, so a captured layout replays verbatim.

  ```lua
  local props = {
      { model = 'ex_prop_adv_case_sm',        position = { x = 0.75, y = 0.70, z = 0.0 }, rotation = 0, data = { id = 'loot_1' } },
      { model = 'ch_prop_ch_crate_empty_01a', position = { x = 2.10, y = 0.80, z = 0.0 }, rotation = 1, data = nil },
  }
  exports['devhub_grid']:PlaceGridProps('warehouse1', props)
  ```

* **DumpGridZone(uid)**: Returns an array of the zone's placed props
  (`{ model, position, rotation, data }`) and prints a copy-pasteable block to the console. Returns
  `nil` if the zone is not known on this client.

* **ActivateEditMode(uid, mode, model, data)**: Opens the interactive editor for the calling player.
  `mode` is `'add'` (also pass `model` and optional `data`), `'remove'`, or `'view'`. Returns the
  placement/removal `status`, and for remove mode the removed prop's `model` and `data`.

***

## <mark style="color:yellow;">**Server Exports**</mark>

Call these from any server script as `exports['devhub_grid']:<name>(...)`.

```lua
exports['devhub_grid']:SetZoneOwner(uid, ownerServerId)   -- bool
```

* **SetZoneOwner(uid, ownerServerId)**: Reassigns the owner of an existing zone to the given
  connected player. The zone and its props are removed when the owner disconnects, so use this to
  hand a scripted zone to whichever player should keep it alive. Returns `true` on success.

***

## <mark style="color:yellow;">**Prop Catalog Helpers**</mark>

Only models present in `gridProps` (ground) / `gridPropsVehicle` (vehicle) in `configs/c.config.lua`
can be placed. Register additional models at runtime from a client script:

```lua
exports['devhub_grid']:AddGridProp(model, data)          -- bool
exports['devhub_grid']:AddGridPropVehicle(model, data)   -- bool
```

* **AddGridProp(model, data)** / **AddGridPropVehicle(model, data)**: Adds (or overwrites) a model in
  the ground / vehicle catalog and returns `true`. `data` describes the prop's footprint:

  ```lua
  {
      size = { x = 0.75, y = 0.70, z = 0.47 },   -- bounding size in meters (required)
      centerOffset = { x = 0.0, y = 0.0, z = 0.0 },  -- optional: shift the box off the model origin
      zOffset = 0.0,                              -- optional: nudge the prop up/down when spawned
      dontAllowStacking = false,                  -- optional: block props from stacking on top
  }
  ```
