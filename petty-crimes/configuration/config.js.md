---
description: Documentation for config.js Configuration
---

# config.js

Found in the resource's `html/` folder. It tunes the NUI (minigames, editor, shop).

```javascript
window.config = {
    numberFormatting: [/\B(?=(\d{3})+(?!\d))/g, " "],
    soundVolume: 0.25,
};
```

## <mark style="color:yellow;">**Number Formatting**</mark>

* **Description**: A `[pattern, replacement]` pair applied to every number shown in the UI. The default groups thousands with a space.
* **Example**: `1500` renders as `1 500`. Use `[/\B(?=(\d{3})+(?!\d))/g, ","]` to render it as `1,500` instead.

***

## <mark style="color:yellow;">**Sound Volume**</mark>

* **Description**: Master volume for the UI sounds (minigame clicks, success/fail stingers). `0` mutes them, `1` is full volume.
* **Example**: `0.25` plays the interface sounds at a quarter of full volume.
