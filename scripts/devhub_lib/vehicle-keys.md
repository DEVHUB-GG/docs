# 🔑 Vehicle Keys

{% hint style="success" %}
File Path: <kbd>modules/systems/c.vehicleKeys.lua</kbd>
{% endhint %}

***

## <mark style="color:yellow;">Pre configured scripts</mark>

* qb-vehiclekeys
* qs-vehiclekeys
* ak47\_vehiclekeys
* t1ger\_keys
* Renewed-Vehiclekeys
* cd\_garage
* custom

***

## <mark style="color:yellow;">Auto detection of resource</mark>

By default, the script will try to automatically detect the correct resource.

```lua
Shared.VehicleKeys = "AUTO DETECT"
```

***

## <mark style="color:yellow;">Auto detection failed</mark>

If auto detect failed in <kbd>config.lua</kbd>, set **Shared.VehicleKeys** to use the appropriate keys resource or set it to custom and configurate it yourself.

***

## <mark style="color:yellow;">Custom functionality</mark>

Go to <kbd>modules/systems/c.vehicleKeys.lua</kbd> and customize your keys logic

```lua
if Shared.VehicleKeys ~= "custom" then
    Core.AddVehicleKeys = function(plate, vehicle)
        -- Add your custom vehicle keys logic here
    end
end
```

