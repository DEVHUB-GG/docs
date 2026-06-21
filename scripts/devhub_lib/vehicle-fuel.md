# ⛽ Vehicle fuel

{% hint style="success" %}
File Path: <kbd>modules/systems/c.vehicleFuel.lua</kbd>
{% endhint %}

***

## <mark style="color:yellow;">Pre configured scripts</mark>

* LegacyFuel
* ps-fuel
* ox\_fuel
* cd\_fuel
* custom

***

## <mark style="color:yellow;">Auto detection of resource</mark>

By default, the script will try to automatically detect the correct resource.

```lua
Shared.VehicleFuel = "AUTO DETECT"
```

***

## <mark style="color:yellow;">Auto detection failed</mark>

If auto detect failed in <kbd>config.lua</kbd>, set **Shared.VehicleFuel** to use the appropriate keys resource or set it to custom and configurate it yourself.

***

## <mark style="color:yellow;">Custom functionality</mark>

Go to <kbd>modules/systems/c.vehicleFuel.lua</kbd> and customize your fuel logic

```lua
Core.SetVehicleFuel = function(vehicle, amount)
    SetVehicleFuelLevel(vehicle, formatFuel(amount)) -- custom or default
end
```

