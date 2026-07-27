---
description: >-
  Ready-made gym layouts for five popular MLOs, so you do not have to place
  anything yourself.
---

# 🗺️ Plug and Play Map Bundles

The `maps_plug_and_play/` folder holds finished gym layouts for five well-known gym MLOs. Each bundle is a small SQL file plus the map files it has to replace. Import it and the whole gym is there: the zone, every machine, the interaction points and the map blip.

You can still open the Gym Creator afterwards and move, add or remove anything.

| Folder | MLO |
| --- | --- |
| `k4mb1-gym` | K4MB1 Gym |
| `lks_gym` | LKS Gym |
| `origen_gymmap` | Origen gym at Vespucci Beach |
| `origen_muscle_gym_sandy_v3` | Origen Muscle Gym, Sandy Shores |
| `prompt_gym` | Prompt Gym, Vinewood |

Every folder also contains an instructions file with the exact steps for that map.

***

## <mark style="color:yellow;">**The usual procedure**</mark>

{% stepper %}
{% step %}
### Open the `stream` folder of your map resource
{% endstep %}

{% step %}
### Replace the file the bundle ships

Most bundles ship a single `.ytyp` or `.ymap` that has to overwrite the one in the map resource. Keep a backup of the original.
{% endstep %}

{% step %}
### Import that bundle's `install.sql`

This is what creates the gym itself.
{% endstep %}

{% step %}
### Restart the server
{% endstep %}
{% endstepper %}

***

## <mark style="color:yellow;">**The two exceptions**</mark>

{% tabs %}
{% tab title="Origen gym map" %}
This bundle ships **two** ymaps: `hei_vb_34_strm_1.ymap` and `jge_musclesands_exterior.ymap`. Replace both in `origen_gymmap\stream\[YMAPs]`.

Then open `devhub_gym2_assets\stream`, find `hei_vb_34_strm_1.ymap` and **delete it**. Both resources ship that file and the map resource has to be the one that loads.
{% endtab %}

{% tab title="Prompt gym" %}
No file replacement here. Open `config.lua` in your `prompt_gym` resource, find `GymLocations`, and in the **vinewood** entry set:

```lua
enabled = false
martialarts = false
weights1 = false
```

That turns off the training features the map ships with, so they do not sit on top of the DevHub equipment.
{% endtab %}
{% endtabs %}

***

{% hint style="info" %}
The bundles are additive. Each `install.sql` inserts its own gym zone, so you can run more than one and end up with several gyms across the map.
{% endhint %}

{% hint style="warning" %}
Nothing in `maps_plug_and_play/` is loaded by the resource itself. The files are there for you to copy into your map resources, and the folder can be deleted once you are done.
{% endhint %}
