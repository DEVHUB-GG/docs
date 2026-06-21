# 🖐️ Target

{% hint style="success" %}
File Path: <kbd>modules/systems/targets</kbd>
{% endhint %}

***

## <mark style="color:yellow;">Pre configured scripts</mark>

* ox\_target
* qb-target
* standalone
* custom

***

## <mark style="color:yellow;">Auto detection of resource</mark>

By default, the script will try to automatically detect the correct resource.

```lua
Shared.Target = "AUTO DETECT"
```

***

## <mark style="color:yellow;">Auto detection failed</mark>

If auto detect failed in <kbd>config.lua</kbd>, set **Shared.Target** to use the appropriate resource or set it to custom and configurate it yourself.

***

## <mark style="color:yellow;">Custom functionality</mark>

Go to <kbd>modules/systems/targets/c.custom.lua</kbd> and customize your logic

```lua
    Core.AddModelToTarget = function(model, data)
        --[[
            This file defines a custom target for the DH framework.
            It contains the following data properties:
            - @data.name: The unique identifier for the target.
            - @data.event: The event that triggers the target.
            - @data.icon: The icon to display for the target (using FontAwesome).
            - @data.label: The label to display on the target.
            - @data.handler: The function to call when the target is interacted with.
        ]]
    end

        Core.AddCoordsToTarget = function(coords, data)
        --[[
            This function adds a spherical target zone at specified coordinates.
            It contains the following data properties:
            - @coords: Vector3 coordinates where to place the target zone
            - @data.name: The unique identifier for the target
            - @data.event: The event that triggers the target
            - @data.icon: The icon to display for the target (using FontAwesome)
            - @data.label: The label to display on the target
            - @data.handler: The function to call when the target is interacted with
            - @data.radius: The radius of the sphere zone
        ]]
        -- Implementation for custom target system would go here
    end
```
