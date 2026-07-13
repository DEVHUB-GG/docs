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
/* YOU CAN'T USE A COMMA BETWEEN RGB */
--primary: 253 209 64; /* #fdd140 */
--background: 32 46 59; /* #202e3b */
--disabled: 114 114 114; /* #727272 */
--background-body: 11 19 32; /* #0b1320 */
--background-light: 42 61 78; /* #2a3d4e */

/*!!! YOU MUST USE A COMMA BETWEEN RGB !!!*/
--primary-rgba: 253, 209, 64; /* #fdd140 */
--grid-dark-rgba: 11, 19, 32; /* #0b1320 */
```

#### To customize the UI for yourself, simply edit the RGB color.

{% hint style="danger" %}
If you are editing the primary color, we recommend changing it in two locations!
{% endhint %}

`--primary` and `--primary-rgba` are the same color written two ways. The first has **no commas**, the second **must** keep them. Change one and forget the other and half the interface keeps the old accent.

Below these, the file carries the full Tailwind palette as ready-made variables (`--red-500`, `--sky-400`, and so on, in every shade from `50` to `950`). Not all of them are used: they are there so you can pick a color from a known-good palette instead of inventing one.

{% hint style="info" %}
This page is about the **interface**: the maker window and the admin panel. It does not touch the crosshair itself. The crosshair's own color, its outline and its dot are picked by each player in the maker, under **Style**.
{% endhint %}
