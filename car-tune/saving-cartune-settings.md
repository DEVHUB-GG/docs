# 💾 Saving Cartune Settings

The `dh_cartune` script provides two exports that allow you to save and apply vehicle tuning settings:

```lua
local tune = exports["dh_cartune"]:getVehicleTune(plate)
exports["dh_cartune"]:setVehicleTune(plate, tune)
```

This is especially useful when storing vehicles in a garage. Typically, vehicle properties should be saved when storing the vehicle and applied when retrieving it.

#### Example Usage

**Storing a Vehicle**

When storing a vehicle, retrieve and save its tuning settings:

```lua
local vehicleProperties = GetVehicleData(vehicle) -- function from garage script
local plate = GetVehicleNumberPlateText(vehicle)
local tune = exports["dh_cartune"]:getVehicleTune(plate)
vehicleProperties.tune = tune
```

**Retrieving a Vehicle**

When retrieving a vehicle, apply the saved tuning settings:

```lua
local vehicleProperties = GetVehicleData(vehicle) -- function from garage script
local plate = GetVehicleNumberPlateText(vehicle)
exports["dh_cartune"]:setVehicleTune(plate, vehicleProperties.tune)
```

This ensures that the vehicle maintains its custom tuning when it is taken out of storage.
