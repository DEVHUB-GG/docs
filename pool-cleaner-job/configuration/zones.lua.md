# zones.lua

{% hint style="warning" %}
Each zone must have trashCoords, pourMixtureCoords, poolCornersCoords, valveCoords
{% endhint %}

```lua
Shared.Zones = {
 {
        trashCoords = { -- vec3
            vec3(158.6598, 453.1288, 140.7271),
            vec3(151.8246, 451.1259, 140.7271),
            vec3(152.0769, 458.2865, 140.7271),
            vec3(159.1990, 454.8471, 140.7271),
        },
        pourMixtureCoords = { -- vec4
            vec4(152.7132, 457.2784, 140.7269, 144.8719),
            vec4(144.0951, 457.1954, 140.7268, 250.2204),
            vec4(150.2169, 452.5387, 140.7269, 321.3112),
            vec4(155.9731, 452.4857, 140.5244, 70.2632),
        },
        poolCornersCoords = { -- vec3
            vec3(145.5005, 460.5179, 140.7269),
            vec3(143.3172, 455.8071, 140.7269),
            vec3(153.9579, 450.6686, 140.7269),
            vec3(155.9381, 455.6248, 140.7269),
        },
        valveCoords = {
            vec4(156.6020, 456.9440, 140.7269, 155.6201),
            vec4(159.1002, 451.6286, 140.7269, 41.4354),
        },
    },
   ...MORE ZONES
}
```
