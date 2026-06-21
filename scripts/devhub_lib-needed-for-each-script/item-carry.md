# 📦 Item Carry

The Item Carry system allows players to physically carry items in their hands with custom props and animations. When a player picks up a configured item, it will appear as a 3D object attached to their character with a carrying animation.

***

### <mark style="color:yellow;">Features</mark>

* **Physical Item Representation**: Items appear as 3D props in the player's hands
* **Custom Animations**: Different carrying animations based on item type
* **Automatic Synchronization**: Items are automatically attached/detached when added/removed from inventory
* **Framework Integration**: Pre-configured for ESX and OX Inventory
* **Prop Generator**: Built-in tool to create custom item configurations
* **Network Synchronization**: Carried items are visible to all players

***

### <mark style="color:yellow;">Supported Frameworks</mark>

#### ✅ **ESX** - Fully Configured

ESX framework is pre-configured and ready to use. The system automatically triggers when items are added or removed from the player's inventory.

#### ✅ **OX Inventory** - Fully Configured

OX Inventory is pre-configured and ready to use out of the box.

#### ⚠️ **QB-Core** - Manual Configuration Required

QB-Core does not have built-in events for item add/remove operations, so manual integration is required.

#### 🔧 **Custom Inventory** - Manual Integration

For custom inventory systems, you need to manually trigger the events when items are added or removed.

***

### <mark style="color:yellow;">Configuration</mark>

#### **Item Configuration (`config.lua`)**

Items are configured in the `Shared.ItemCarry` table:

```lua
Shared.ItemCarry = {
    ["item_name"] = { 
        prop = "prop_model_name",                              -- Prop model to spawn
        offset = vec3(0.0855, 0.31115, 0.248),                -- Position offset from player
        rotation = vec3(-72.99, -78.20, -9.89),               -- Rotation of the prop
        anim = "1"                                             -- Animation type (1 or 2)
    },
}
```

**Example Configurations:**

```lua
Shared.ItemCarry = {
    ["dh_hr_tv"] = { 
        prop = "ex_prop_ex_tv_flat_01", 
        offset = vec3(0.0855, 0.31115, 0.248), 
        rotation = vec3(-72.99, -78.20, -9.89), 
        anim = "1" 
    },
    ["dh_hr_laptop"] = { 
        prop = "xm_prop_x17_laptop_mrsr", 
        offset = vec3(0.2125, -0.015, 0.2075), 
        rotation = vec3(-55.5, -67.5, 0.5), 
        anim = "1" 
    },
    ["dh_hr_guitar"] = { 
        prop = "prop_el_guitar_01", 
        offset = vec3(0.06, 0.025, 0.1175), 
        rotation = vec3(-12.5, 34.0, 159.5), 
        anim = "1" 
    },
}
```

#### **Animation Types**

```lua
Animations = {
    ["1"] = {
        "anim@heists@box_carry@", "idle"    -- Standard box carrying animation
    },
    ["2"] = {
        "impexp_int-0", "mp_m_waremech_01_dual-0"  -- Alternative carrying animation
    },
}
```

***

### <mark style="color:yellow;">QB-Core Integration</mark>

Since QB-Core doesn't have built-in events for inventory operations, you need to manually add triggers to your `qb-inventory` resource.

#### **Step 1: Locate the Functions**

Navigate to `qb-inventory/server/functions.lua`

#### **Step 2: Add Event Triggers**

Find the `AddItem` function and add the following line **before the return statement**:

```lua
function AddItem(identifier, item, amount, metadata, slot)
    -- ... existing code ...
    
    -- Add this line BEFORE the return statement
    TriggerClientEvent("devhub_lib:itemCarry:addItem:client", QBCore.Functions.GetSource(identifier), item)
    
    return true
end
```

Find the `RemoveItem` function and add the following line **before the return statement**:

```lua
function RemoveItem(identifier, item, amount, slot, metadata)
    -- ... existing code ...
    
    -- Add this line BEFORE the return statement
    TriggerClientEvent("devhub_lib:itemCarry:removeItem:client", QBCore.Functions.GetSource(identifier), item)
    
    return true
end
```

#### **Complete Example:**

```lua
-- qb-inventory/server/functions.lua

function AddItem(identifier, item, amount, metadata, slot)
    local source = QBCore.Functions.GetSource(identifier)
    local Player = QBCore.Functions.GetPlayer(source)
    
    if not Player then return false end
    
    -- ... existing item adding logic ...
    
    -- Trigger item carry event
    TriggerClientEvent("devhub_lib:itemCarry:addItem:client", source, item)
    
    return true
end

function RemoveItem(identifier, item, amount, slot, metadata)
    local source = QBCore.Functions.GetSource(identifier)
    local Player = QBCore.Functions.GetPlayer(source)
    
    if not Player then return false end
    
    -- ... existing item removal logic ...
    
    -- Trigger item carry event
    TriggerClientEvent("devhub_lib:itemCarry:removeItem:client", source, item)
    
    return true
end
```

***

### <mark style="color:yellow;">Custom Inventory Integration</mark>

If you're using a custom inventory system, you need to manually trigger the item carry events.

#### **When Adding an Item:**

```lua
-- Trigger this event when a player receives an item
TriggerClientEvent("devhub_lib:itemCarry:addItem:client", playerId, itemName)
```

#### **When Removing an Item:**

```lua
-- Trigger this event when a player loses an item
TriggerClientEvent("devhub_lib:itemCarry:removeItem:client", playerId, itemName)
```

#### **Example Implementation:**

```lua
-- In your custom inventory's server-side code

function YourInventory.AddItem(playerId, itemName, amount)
    -- Your existing add item logic
    
    -- Trigger item carry event
    TriggerClientEvent("devhub_lib:itemCarry:addItem:client", playerId, itemName)
end

function YourInventory.RemoveItem(playerId, itemName, amount)
    -- Your existing remove item logic
    
    -- Trigger item carry event
    TriggerClientEvent("devhub_lib:itemCarry:removeItem:client", playerId, itemName)
end
```

***

### <mark style="color:yellow;">Prop Generator</mark>

The prop generator is a powerful tool that helps you create item carry configurations without manual trial and error.

#### **Enabling the Generator**

To use the prop generator, you must enable **Development Mode** in `config.lua`:

```lua
Shared.DevelopmentMode = true  -- Set to true to enable the generator
```

⚠️ **WARNING**: Always set this to `false` in production environments!

***

#### **Using the Prop Generator**

**Step 1: Enable Development Mode**

```lua
Shared.DevelopmentMode = true
```

**Step 2: In-Game Command**

Use the following command in-game:

```
/propgenerator [item_name] [prop_model]
```

**Parameters:**

* `item_name`: The name of your item (e.g., `dh_hr_box`)
* `prop_model`: The GTA V prop model name (e.g., `prop_box_wood01a`)

**Example:**

```
/propgenerator dh_hr_box prop_box_wood01a
```

***

#### **Generator Controls**

Once the generator is activated, you'll see an on-screen menu with the following controls:

| Control         | Button | Action                          |
| --------------- | ------ | ------------------------------- |
| **Arrow Up**    | ⬆️     | Move to previous parameter      |
| **Arrow Down**  | ⬇️     | Move to next parameter          |
| **Arrow Left**  | ⬅️     | Decrease value                  |
| **Arrow Right** | ➡️     | Increase value                  |
| **Enter**       | ↵      | Save configuration to clipboard |
| **ESC**         | Esc    | Cancel and close generator      |

**Available Parameters:**

1. **X Offset**: Horizontal position offset (-10.0 to 10.0)
2. **Y Offset**: Depth position offset (-10.0 to 10.0)
3. **Z Offset**: Vertical position offset (-10.0 to 10.0)
4. **Rotation X**: Pitch rotation (-180° to 180°)
5. **Rotation Y**: Roll rotation (-180° to 180°)
6. **Rotation Z**: Yaw rotation (-180° to 180°)
7. **Step Size**: Adjustment precision (0.01, 0.1, 0.2, 0.5, 1.0)

***

#### **Generator Workflow**

1. **Activate Generator**: Use `/propgenerator item_name prop_model`
2. **Adjust Position**: Use arrow keys to move through parameters and adjust values
3. **Fine-tune**: Change step size for more precise adjustments
4. **Save Configuration**: Press **Enter** when satisfied with the positioning
5. **Paste Configuration**: The config is automatically copied to your clipboard
6. **Add to Config**: Paste the configuration into `config.lua` in the `Shared.ItemCarry` table
7. **Disable Development Mode**: Set `Shared.DevelopmentMode = false` before going to production

***

#### **Generator Output Example**

When you press **Enter**, the generator will copy this format to your clipboard:

```lua
["dh_hr_box"] = { prop = "prop_box_wood01a", offset = vec3(0.15, 0.20, 0.25), rotation = vec3(-45.0, 10.0, 5.0), anim = "1" }
```

Simply paste this line into your `Shared.ItemCarry` table in `config.lua`.

***

### <mark style="color:yellow;">Exports</mark>

#### **Client-Side Exports**

**Check if Player is Carrying an Item**

```lua
local isCarrying = exports.devhub_lib:IsPlayerCarryingItem()

if isCarrying then
    print("Player is carrying an item")
else
    print("Player is not carrying any item")
end
```

**Returns:**

* `true` if player is currently carrying an item
* `false` if player is not carrying anything

***

#### **Server-Side Exports**

**Check if Player is Carrying an Item (Server)**

```lua
local isCarrying = exports.devhub_lib:IsPlayerCarryingItem(playerId)

if isCarrying then
    print("Player " .. playerId .. " is carrying an item")
end
```

**Parameters:**

* `playerId` (number): The server ID of the player

**Returns:**

* `true` if player is currently carrying an item
* `false` if player is not carrying anything

***

### <mark style="color:yellow;">Events</mark>

#### **Client Events**

**Add Item to Player**

```lua
TriggerEvent("devhub_lib:itemCarry:addItem:client", itemName)
```

**Remove Item from Player**

```lua
TriggerEvent("devhub_lib:itemCarry:removeItem:client", itemName)
```

**Force Delete Carried Entity**

```lua
TriggerEvent("devhub_lib:itemCarry:deleteEntity")
```

#### **Server Events**

These events are automatically triggered by the system and should not be called manually unless integrating with a custom inventory system.

***

### <mark style="color:yellow;">Routing Buckets Synchronization</mark>

The Item Carry system automatically synchronizes carried props with routing buckets to ensure proper visibility across different dimensions/instances.

#### **What are Routing Buckets?**

Routing buckets are FiveM's way of creating separate "dimensions" or "instances" where players in different buckets cannot see or interact with each other. This is commonly used for:

* Instanced apartments/houses
* Separate job areas
* Minigames or events
* Private zones

#### **Why Bucket Synchronization is Important**

When a player carrying an item is moved to a different routing bucket, the carried prop entity must also be moved to the same bucket. Otherwise, the prop will remain in the old bucket and won't be visible to the player or others in the new bucket.

#### **Automatic Synchronization**

The system listens for bucket changes through the following event:

```lua
RegisterNetEvent("devhub_lib:server:setBucket", function(source, bucket)
    -- Automatically syncs carried item to new bucket
end)
```

#### **Manual Integration**

If you're using a custom bucket system or other resources that change player routing buckets, you need to trigger this event whenever you move a player to a different bucket.

**Server-Side Event Trigger**

When changing a player's routing bucket, trigger the synchronization event:

```lua
-- Example: Moving player to a new bucket
local playerId = source
local newBucket = 10

-- Set player's routing bucket
SetPlayerRoutingBucket(playerId, newBucket)

-- IMPORTANT: Sync the carried item to the new bucket
TriggerEvent("devhub_lib:server:setBucket", playerId, newBucket)
```

**Complete Example - Housing System**

```lua
-- When player enters an apartment
RegisterNetEvent('housing:enterApartment', function(apartmentId)
    local source = source
    local bucket = apartmentId + 1000  -- Unique bucket for each apartment
    
    -- Move player to apartment bucket
    SetPlayerRoutingBucket(source, bucket)
    SetEntityRoutingBucket(GetPlayerPed(source), bucket)
    
    -- Sync carried items to new bucket
    TriggerEvent("devhub_lib:server:setBucket", source, bucket)
    
    -- Teleport player
    SetEntityCoords(GetPlayerPed(source), apartmentCoords.x, apartmentCoords.y, apartmentCoords.z)
end)

-- When player leaves apartment
RegisterNetEvent('housing:exitApartment', function()
    local source = source
    
    -- Return player to main world (bucket 0)
    SetPlayerRoutingBucket(source, 0)
    SetEntityRoutingBucket(GetPlayerPed(source), 0)
    
    -- Sync carried items to main world
    TriggerEvent("devhub_lib:server:setBucket", source, 0)
    
    -- Teleport player outside
    SetEntityCoords(GetPlayerPed(source), exitCoords.x, exitCoords.y, exitCoords.z)
end)
```

#### **Important Notes**

⚠️ **Always trigger the sync event** whenever you change a player's routing bucket if they might be carrying items.

⚠️ **Order matters**: You can trigger the sync event before or after changing the bucket, but it's recommended to do it immediately after.

⚠️ **Bucket 0 is the default**: The main world is always bucket 0. Return players to bucket 0 when they exit instances.

#### **Common Bucket Systems**

If you're using popular housing or instance systems, here's how to integrate:

**QB-Apartments / QB-Housing**

Add the sync event in the apartment entry/exit functions:

```lua
-- In qb-apartments/server/main.lua or similar
TriggerEvent("devhub_lib:server:setBucket", source, bucket)
```

**ESX Property**

Add the sync event when entering/exiting properties:

```lua
-- In esx_property or similar
TriggerEvent("devhub_lib:server:setBucket", source, bucket)
```

**Custom Instance Systems**

Whenever you call `SetPlayerRoutingBucket()`, add:

```lua
TriggerEvent("devhub_lib:server:setBucket", playerId, bucket)
```



### <mark style="color:yellow;">Troubleshooting</mark>

#### **Item not appearing when picked up:**

* Verify the item name in `Shared.ItemCarry` matches your inventory item name exactly
* Check that the prop model exists in GTA V
* Enable development mode and check console for errors
* Ensure `Shared.ItemCarry` is properly configured

#### **Prop spawning in wrong position:**

* Use the prop generator to adjust offset and rotation values
* Fine-tune with smaller step sizes (0.01 or 0.1)
* Check animation type (anim = "1" or "2") for different carry styles

#### **Animation not playing:**

* Verify the animation dictionary exists
* Check if `anim` value is set to "1" or "2"
* Try switching between animation types

#### **QB-Core not working:**

* Ensure you added the event triggers to BOTH `AddItem` and `RemoveItem` functions
* Verify the triggers are placed BEFORE the return statements
* Check server console for errors
* Restart `qb-inventory` after making changes

#### **Prop generator not working:**

* Ensure `Shared.DevelopmentMode = true` in `config.lua`
* Restart the resource after changing development mode
* Use valid GTA V prop model names
* Check console for spawn errors

#### **Multiple items conflicting:**

* Players can only carry one item at a time
* If carrying an item, picking up another won't work until the first is removed

***

### <mark style="color:yellow;">Best Practices</mark>

1. **Always test in development mode first** before deploying to production
2. **Use the prop generator** instead of manually guessing offset/rotation values
3. **Disable development mode** in production (`Shared.DevelopmentMode = false`)
4. **Backup your config** before making bulk changes

***

### <mark style="color:yellow;">Performance Considerations</mark>

* The system automatically cleans up props when items are removed
* Carried items are synchronized across all clients
* Props are deleted when players disconnect
* Only one item can be carried at a time to prevent performance issues

***

### <mark style="color:yellow;">Example Use Cases</mark>

#### **Delivery System**

```lua
-- When player accepts delivery
TriggerClientEvent("devhub_lib:itemCarry:addItem:client", playerId, "dh_hr_package")

-- When player completes delivery
TriggerClientEvent("devhub_lib:itemCarry:removeItem:client", playerId, "dh_hr_package")
```

#### **Robbery System**

```lua
-- When player steals item
TriggerClientEvent("devhub_lib:itemCarry:addItem:client", playerId, "dh_hr_tv")

-- Check if player can run
if exports.devhub_lib:IsPlayerCarryingItem(playerId) then
    -- Slow down player movement
end
```

#### **Moving System**

```lua
-- Multiple items for moving house
Shared.ItemCarry = {
    ["moving_box"] = { prop = "prop_cardbordbox_02a", offset = vec3(0.0, 0.3, 0.2), rotation = vec3(0.0, 0.0, 0.0), anim = "1" },
    ["moving_sofa"] = { prop = "prop_couch_lg_02", offset = vec3(0.5, 0.1, 0.0), rotation = vec3(-90.0, 0.0, 0.0), anim = "1" },
    ["moving_tv"] = { prop = "prop_tv_flat_01", offset = vec3(0.1, 0.3, 0.25), rotation = vec3(-72.0, -78.0, -10.0), anim = "1" },
}
```
