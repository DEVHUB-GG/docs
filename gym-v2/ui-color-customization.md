---
description: How to recolor the Gym V2 interface.
---

# 🎨 UI Color Customization

## <mark style="color:yellow;">Usage</mark>

{% stepper %}
{% step %}
### Enter the `devhub_gym2` folder
{% endstep %}

{% step %}
### Enter the `html` folder
{% endstep %}

{% step %}
### Open the `colors.css` file
{% endstep %}
{% endstepper %}

#### The part you edit looks like this

```css
:root {
    /* custom colors */

    /* Do not put commas between the RGB components. */
    --primary: 253 209 64; /* #fdd140 */
    --background: 32 46 59; /* #202e3b */
    --background-border: 49 71 90; /* #31475a */

    /* This variable must have commas between the RGB components. */
    --primary-rgba: 253, 209, 64; /* #fdd140 */
}
```

#### To customize the UI for yourself, simply edit the RGB color.

These four variables color everything the script draws: the gym menu, the Gym Creator, the business panel, the admin panel and the minigames.

* **--primary**: the accent color. Highlights, active tabs, buttons, progress bars, the dumbbell icons.
* **--background**: panels and cards.
* **--background-border**: the borders and dividers between them.
* **--primary-rgba**: the same accent color again, used wherever transparency is needed (glows, gradients, shadows).

{% hint style="danger" %}
Watch the format, the two groups are **not** written the same way. The first group uses **spaces** between the RGB values (`--primary: 253 209 64;`), and `--primary-rgba` uses **commas** (`--primary-rgba: 253, 209, 64;`). Mixing them up breaks the UI colors.

When you change `--primary`, change `--primary-rgba` to the same color.
{% endhint %}

{% hint style="info" %}
The file also ships the Tailwind palette (gray, red, orange, green, blue and so on) as reference variables below the custom colors. Those are the fixed semantic colors: green for confirm, red for delete. You normally only touch the four variables above.
{% endhint %}

{% hint style="success" %}
No rebuild is needed. Edit the file, restart the resource, and the interface comes back in the new colors.
{% endhint %}
