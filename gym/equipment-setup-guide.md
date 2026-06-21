# 💪 Equipment Setup Guide

{% hint style="success" %}
**TL;DR** — Open `configs/shared.lua`, add a new entry inside `Shared.Exercises`, restart the resource. Done.
{% endhint %}

### Where to edit

All equipment is configured in a single file:

{% code title="configs/shared.lua" %}
```lua
Shared.Exercises = {
    -- your entries go here
}
```
{% endcode %}

***

### Available exercise UIDs

Each entry must use one of these `uid` values (defined in `Shared.ExercisesTypes`):

<table><thead><tr><th width="220">UID</th><th>Exercise</th></tr></thead><tbody><tr><td><code>kettlebellswing</code></td><td>Kettlebell swing</td></tr><tr><td><code>boxing</code></td><td>Punching bag</td></tr><tr><td><code>jumpingbox</code></td><td>Box jumps</td></tr><tr><td><code>overhead</code></td><td>Overhead press (barbell)</td></tr><tr><td><code>backsquat</code></td><td>Back squat (barbell)</td></tr><tr><td><code>raises</code></td><td>Dumbbell raises (large rack)</td></tr><tr><td><code>biceps</code></td><td>Biceps curls (small rack)</td></tr><tr><td><code>treadmill</code></td><td>Treadmill run</td></tr></tbody></table>

{% hint style="info" %}
Each `uid` is linked to `Shared.ExercisesTypes` (minigames + skill tree XP). To add a brand-new exercise type, you must extend `Shared.ExercisesTypes` **and** create the matching exercise script in `escrowed/exercises/`.
{% endhint %}

***

### Available props (known models)

These models have a default menu offset registered in `Config.PropsMenuOffset` (`configs/client.lua`):

<table><thead><tr><th width="280">Model</th><th>Used for</th></tr></thead><tbody><tr><td><code>devhub_gym_kettlebell_rack</code></td><td>Kettlebells</td></tr><tr><td><code>devhub_gym_punch_bag</code></td><td>Punching bag</td></tr><tr><td><code>devhub_gym_jumping_box</code></td><td>Jump box</td></tr><tr><td><code>devhub_gym_barbell</code></td><td>Barbell — squat</td></tr><tr><td><code>devhub_gym_dumbell2_rack</code></td><td>Large dumbbells</td></tr><tr><td><code>devhub_gym_dumbell1_rack</code></td><td>Small dumbbells</td></tr><tr><td><code>prop_barbell_02</code></td><td>Vanilla GTA barbell — overhead</td></tr><tr><td><code>devhub_gym_treadmill</code></td><td>Treadmill</td></tr></tbody></table>

***

### How to add new equipment (script spawns the prop)

{% stepper %}
{% step %}
#### Pick a UID and prop

Choose from the tables above.
{% endstep %}

{% step %}
#### Get the coordinates

Stand where you want the prop in-game, copy `x, y, z, heading`.
{% endstep %}

{% step %}
#### Add the entry

Append a new table inside `Shared.Exercises` in `configs/shared.lua`:

```lua
{
    uid = "boxing",                  -- exercise type from the list above
    maxReps = 10,                    -- max reps per session
    placeOnTheGround = true,         -- auto-snap prop to the ground
    propName = "devhub_gym_punch_bag",
    propCoords = vec4(x, y, z, h),   -- prop position
    playerCoords = vec4(x, y, z, h), -- (optional) where the player stands
},
```
{% endstep %}

{% step %}
#### Restart

```bash
restart devhub_gym
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
`playerCoords` is **not required** for every exercise (e.g. `boxing`, `jumpingbox`, `treadmill` don't use it), but it is **recommended** for `kettlebellswing`, `overhead`, `backsquat`, `raises`, `biceps`.
{% endhint %}

***

### How to use an EXISTING prop from the map (MLO / IPL)

If the prop already exists in the world and you do not want to spawn a duplicate, set `dontSpawnProp = true`:

{% code title="configs/shared.lua" %}
```lua
{
    uid = "boxing",
    maxReps = 10,
    dontSpawnProp = true,            -- IMPORTANT — skip spawning, use the existing prop
    propName = "devhub_gym_punch_bag",
    propCoords = vec4(x, y, z, h),   -- coords of the existing prop
},
```
{% endcode %}

{% hint style="warning" %}
`propCoords` **must match** the real position of the in-world prop. The script uses it to place the interaction menu and to position the player.
{% endhint %}

***

### Full entry options

<table><thead><tr><th width="180">Key</th><th width="100">Type</th><th width="100" align="center">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>uid</code></td><td><code>string</code></td><td align="center">✓</td><td>Exercise type</td></tr><tr><td><code>maxReps</code></td><td><code>number</code></td><td align="center">✓</td><td>Max reps per session</td></tr><tr><td><code>propName</code></td><td><code>string</code></td><td align="center">✓</td><td>Prop model name</td></tr><tr><td><code>propCoords</code></td><td><code>vec4</code></td><td align="center">✓</td><td>x, y, z, heading</td></tr><tr><td><code>playerCoords</code></td><td><code>vec4</code></td><td align="center">—</td><td>Optional — fixed player position</td></tr><tr><td><code>placeOnTheGround</code></td><td><code>bool</code></td><td align="center">—</td><td><code>true</code> = snap prop to ground</td></tr><tr><td><code>dontSpawnProp</code></td><td><code>bool</code></td><td align="center">—</td><td><code>true</code> = use an existing prop from the map</td></tr></tbody></table>

***

### Important rules

{% hint style="danger" %}
**Do NOT place multiple entries with the same `uid` too close to each other.** Author note in `shared.lua`: _"dont place the same uid too close to each other"_.
{% endhint %}

{% hint style="warning" %}
**`Config.PropSpawnDistance = 50.0` (max `75.0`).** Above that limit, props will not load reliably.
{% endhint %}

{% hint style="info" %}
**Custom prop?** Register its menu offset in `configs/client.lua` → `Config.PropsMenuOffset`, otherwise the menu may appear inside the model:

```lua
Config.PropsMenuOffset = {
    [`your_prop`] = vec3(x, y, z),
}
```
{% endhint %}

{% hint style="info" %}
**Map blips** are configured in `configs/client.lua` → `Config.Blips`.
{% endhint %}

{% hint style="info" %}
**Skill Tree XP** — if `Shared.DevhubSkillTreeEnabled = true`, XP rewards are configured in `Shared.ExercisesTypes[uid].skillTrees`.
{% endhint %}

***

### Examples

{% tabs %}
{% tab title="Existing MLO prop" %}
Punching bag attached to a prop that already exists inside an MLO:

```lua
{
    uid = "boxing",
    maxReps = 15,
    dontSpawnProp = true,
    propName = "prop_box_bag01a",
    propCoords = vec4(150.25, -1005.75, 29.30, 90.0),
},
```
{% endtab %}

{% tab title="Script-spawned treadmill" %}
A new treadmill that the script will spawn for you:

```lua
{
    uid = "treadmill",
    maxReps = 20,
    placeOnTheGround = true,
    propName = "devhub_gym_treadmill",
    propCoords = vec4(-1193.44, -1571.47, 3.61, 214.39),
},
```
{% endtab %}

{% tab title="Back squat with player snap" %}
Forces the player into a fixed spot behind the barbell:

```lua
{
    uid = "backsquat",
    maxReps = 10,
    placeOnTheGround = true,
    propName = "devhub_gym_barbell",
    propCoords   = vec4(-1209.80, -1564.87, 3.82, 122.79),
    playerCoords = vec4(-1208.65, -1564.23, 4.60, 299.04),
},
```
{% endtab %}
{% endtabs %}

***

{% hint style="success" %}
After adding entries, just run **`restart devhub_gym`** on the server.
{% endhint %}
