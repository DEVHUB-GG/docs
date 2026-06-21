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

    /*!!! YOU MUST USE A COMMA BETWEEN RGB !!!*/
    --primary-rgba: 253, 209, 64; /* #fdd140 */
    --grid-dark-rgba: 11, 19, 32; /* #0b1320 */
}
```

#### To customize the UI for yourself, simply edit the RGB color.

{% hint style="danger" %}
The primary color is defined in **two** places — `--primary` (space-separated, no commas) and `--primary-rgba` (comma-separated). If you change the primary color, update **both** so it stays consistent across the UI.
{% endhint %}

{% hint style="info" %}
The file also ships the full Tailwind color palette (slate, gray, red, blue, etc.) as reference variables. Not all of them are used — the comment block marks where the actively used colors end. You normally only need to edit `--primary`, `--primary-rgba`, and `--grid-dark-rgba`.
{% endhint %}
