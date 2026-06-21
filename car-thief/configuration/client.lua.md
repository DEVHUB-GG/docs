---
description: Documentation for client.lua Configuration
---

# client.lua

This file is the **client-side police alarm hook**. It is intentionally left open so you can plug Car Thief into whatever dispatch system your server uses.

## <mark style="color:yellow;">**TriggerAlarm**</mark>

```lua
function TriggerAlarm(coords, plate)
    -- TODO implement your police call logic here (client side) or go to
    -- configs/server.lua to implement it server side

    TriggerServerEvent("devhub_carThief:server:triggerAlarm", coords, plate)
end
```

* **Description**: Called whenever the heist should alert the police — when the thief is caught, and when the thief gets into the stolen vehicle. Add your client-side dispatch call (e.g. a notification, a blip, a `ps-dispatch` / `cd-dispatch` trigger) inside this function.
* **Parameters**:
  * **coords** (`vector3`): where the alarm was raised.
  * **plate** (`string`): license plate of the stolen vehicle (may be `nil`).
* **Note**: By default it forwards the alarm to the server (`devhub_carThief:server:triggerAlarm`) so you can also handle it in [`server.lua`](server.lua.md). Keep this line if you handle dispatch server-side.

***
