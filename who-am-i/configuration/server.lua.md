---
description: Documentation for server.lua Configuration
---

# server.lua

***

## <mark style="color:yellow;">**Main Configuration Table**</mark>

* The `Config` table holds all key configuration settings. Below is an explanation of each parameter and its purpose.

#### **Parameters**

| Parameter        | Type    | Default Value | Description                                                              |
| ---------------- | ------- | ------------- | ------------------------------------------------------------------------ |
| `ItemEnabled`    | boolean | `true`        | Determines if the item functionality is enabled.                         |
| `Item`           | string  | `"whoami"`    | The name of the item. Used only if `ItemEnabled` is set to `true`.       |
| `CommandEnabled` | boolean | `false`       | Determines if the command functionality is enabled.                      |
| `Command`        | string  | `"whoami"`    | The name of the command. Used only if `CommandEnabled` is set to `true`. |

***

## <mark style="color:yellow;">**Example Configuration**</mark>

* Below is an example of how the configuration may look in your `server.lua` file:

```lua
Config = {}

-- Enable or disable item functionality
Config.ItemEnabled = true -- Set to false to disable item functionality

-- Name of the item (used only if ItemEnabled is true)
Config.Item = "whoami"

-- Enable or disable command functionality
Config.CommandEnabled = false -- Set to true to enable the command functionality

-- Name of the command (used only if CommandEnabled is true)
Config.Command = "whoami"
```
