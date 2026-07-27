---
description: Documentation for client.lua Configuration
---

# client.lua

Client-side tunables. The defaults are good for almost every server, and the only one you are likely to change is the sound volume.

## <mark style="color:yellow;">**Sound Volumes**</mark>

```lua
Config.SoundVolumes = {
    master  = 1.0,  -- global volume multiplier
    click   = 0.35, -- keypress sound in minigames
    start   = 0.6,  -- countdown "GO!" sound
    success = 0.7,  -- minigame won
    fail    = 0.7,  -- minigame lost
}
```

* **Description**: Volume of the interface sounds, from `0` (muted) to `1` (full).
* **master**: multiplies all of the others. Set it to `0` to mute the script's sounds entirely without touching the individual values.
* **Example**: `click = 0.15` makes the minigame keypresses much quieter while leaving the win and lose stingers loud.

***

## <mark style="color:yellow;">**Global Models**</mark>

```lua
Config.GlobalModels = {
    range      = 75.0, -- detection range (m)
    intervalMs = 3000, -- scan interval (ms)
}
```

* **Description**: How the script finds props that were enabled on the [global-models.md](../gym-creator/global-models.md) page.
* **range**: how far from the player it looks for those props.
* **intervalMs**: how often it looks.

{% hint style="warning" %}
Both values cost performance on every client. Raising the range or lowering the interval makes global props appear a little faster and costs a lot more. If you use no global models at all, these values do nothing.
{% endhint %}

***

## <mark style="color:yellow;">**Interaction Points**</mark>

```lua
Config.InteractionPoints = {
    spawnDistance      = 75.0,  -- spawn within this radius (m)
    despawnDistance    = 130.0, -- despawn past this radius (m)
    intervalMs         = 1500,  -- normal loop interval (ms)
    farIdleMs          = 4000,  -- loop interval when far away (ms)
    farIdleMultiplier  = 2.0,   -- "far away" = despawnDistance * this
}
```

* **Description**: Controls when the props, peds and targets of interaction points appear and disappear around the player.
* **spawnDistance / despawnDistance**: the gap between the two is deliberate. It stops props popping in and out when a player stands exactly on the boundary.
* **farIdleMs / farIdleMultiplier**: once the player is more than `despawnDistance * farIdleMultiplier` away, the loop slows down to `farIdleMs`, because there is nothing nearby to manage.

***

## <mark style="color:yellow;">**Ped Clear**</mark>

```lua
Config.PedClear = {
    intervalMs   = 5000, -- loop interval (ms)
    edgeBufferM  = 5.0,  -- only clear when player is within this distance of the area edge (m)
}
```

* **Description**: Backs the ped-clearing areas configured on the [other.md](../gym-creator/other.md) tab of a zone.
* **intervalMs**: how often NPCs inside the areas are cleared.
* **edgeBufferM**: clearing only runs when the player is inside the area or within this many meters of its edge, so a gym on the far side of the map costs nothing.
