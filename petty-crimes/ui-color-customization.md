---
description: How to recolor the Petty Crimes interface.
---

# 🎨 UI Color Customization

## <mark style="color:yellow;">Usage</mark>

{% stepper %}
{% step %}
### Enter the folder with the script
{% endstep %}

{% step %}
### Enter `html` folder
{% endstep %}

{% step %}
### Open the `colors.css` file
{% endstep %}
{% endstepper %}

#### The code looks like this

```css
:root {
    /* custom colors */

    /* YOU CAN'T USE A COMMA BETWEEN RGB */
    --primary: 253 209 64; /* #fdd140 */
    --background: 32 46 59; /* #202e3b */
    --disabled: 114 114 114; /* #727272 */
    --background-body: 11 19 32; /* #0b1320 */
    --background-light: 42 61 78; /* #2a3d4e */

    /*!!! YOU MUST USE A COMMA BETWEEN RGB !!!*/
    --primary-rgba: 253, 209, 64; /* #fdd140 */
    --grid-dark-rgba: 11, 19, 32; /* #0b1320 */
}
```

#### To customize the UI for yourself, simply edit the RGB color.

These colors are used by everything the script draws: the minigames, the in-game editor and the dealer's shop dialogue.

* **--primary**: The accent color — highlights, active states, buttons, progress.
* **--background**: Panels and cards.
* **--background-body**: The darkest layer, behind everything.
* **--background-light**: Raised surfaces (inputs, list rows, tabs).
* **--disabled**: Inactive/locked elements.
* **--primary-rgba** / **--grid-dark-rgba**: The same accent and dark tone used where transparency is needed (glows, grid overlays).

{% hint style="danger" %}
Watch the format — the two groups are **not** written the same way. The first group uses **spaces** between the RGB values (`--primary: 253 209 64;`), the `*-rgba` group uses **commas** (`--primary-rgba: 253, 209, 64;`). Mixing them up breaks the UI colors.

When you change `--primary`, change `--primary-rgba` to the same color, and keep `--grid-dark-rgba` in sync with `--background-body`.
{% endhint %}

{% hint style="info" %}
The file also ships the full Tailwind palette (slate, gray, red, blue, …) as reference variables below the custom colors. Not all of them are used — the comment block marks where the actively used colors end. You normally only need to edit the seven variables shown above.
{% endhint %}
