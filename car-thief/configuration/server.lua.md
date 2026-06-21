---
description: Documentation for server.lua Configuration
---

# server.lua

This file is the **server-side police alarm hook**. Use it to integrate Car Thief with a server-side dispatch system.

## <mark style="color:yellow;">**devhub\_carThief:server:triggerAlarm**</mark>

```lua
RegisterNetEvent("devhub_carThief:server:triggerAlarm", function(coords, plate)
    local source = source

    -- TODO implement your server-side police call logic here
end)
```

* **Description**: Fires whenever the alarm is raised on a client — when the thief is caught, and when they get into the stolen vehicle. Add your server-side dispatch integration here (ps-dispatch, cd_dispatch, a native ESX/QB police alert, a Discord log, …).
* **Parameters**:
  * **source**: the player who raised the alarm.
  * **coords** (`vector3`): where the alarm was raised.
  * **plate** (`string`): license plate of the stolen vehicle (may be `nil`).

***
