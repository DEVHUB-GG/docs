# ⚙️ Configuration

1. Download resource from our [github](https://github.com/DEVHUB-GG/dh_lib)
2. Open shared.lua
3. Adjust script to server frameworks

{% code title="shared.lua" %}
```lua
-- This module contains shared configuration settings for the devhub_lib.
-- vRP support is currently in beta. Please report any issues you encounter, check before using in production environments.

-- The framework being used. Possible frameworks: "AUTO DETECT", "ESX", "QBCore", "QBOX" , "VRP" , "custom"
Shared.Framework = "AUTO DETECT"

-- The target system being used. Possible targets: "AUTO DETECT", "ox_target", "qb-target", "standalone", "custom" 
Shared.Target = "AUTO DETECT"

Shared.CompatibilityTest = true -- Enable compatibility tests for the selected framework. !!WARNING!! This must be turned off in production environments.
-- 1. Add new item dh_test
-- 2. Restart your server
-- 3. Join the server and type /dh_startTest to start the test
-- 4. Use item dh_test
-- 5. Revive yourself when you die
-- 6. You will be kicked from the server
-- 7. Check the server console for the test results
```
{% endcode %}





