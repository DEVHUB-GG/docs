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
    --secondary: 126 104 34; /* #7E6822 */
    --background: 1 2 1; /* #010201 */
    --second-background: 15 12 4; /* #0F0C04 */
    --overlay: 7 7 7; /* #070707 */
}
```

#### To customize the UI for yourself, simply edit the RGB color.

{% hint style="danger" %}
Use spaces between the RGB values — **not** commas. `--primary: 253 209 64;` is correct, `253, 209, 64` is not.
{% endhint %}

{% hint style="info" %}
The file also ships the full Tailwind color palette (slate, gray, red, blue, etc.) as reference variables below the custom colors. Not all of them are used — the comment block marks where the actively used colors end. You normally only need to edit `--primary`, `--secondary`, `--background`, `--second-background` and `--overlay`.
{% endhint %}
