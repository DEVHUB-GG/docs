---
description: >-
  This document describes all configuration options for the DevHub Truck robbery
  resource.
---

# ⚙️ Exports & Events

### <mark style="color:yellow;">Client Exports</mark>

#### Close Crafting UI

```lua
exports['devhub_crafting']:Close()
```

Closes the crafting UI if it's currently open. Useful for:

* Closing crafting when player dies
* Closing when player enters a vehicle
* Integration with other UI systems

**Example Usage:**

```lua
-- Close crafting when player dies
AddEventHandler('esx:onPlayerDeath', function()
    exports['devhub_crafting']:Close()
end)

-- Close crafting when entering vehicle
AddEventHandler('baseevents:enteredVehicle', function()
    exports['devhub_crafting']:Close()
end)
```

***

### <mark style="color:yellow;">Server Events</mark>

#### Community Project Completed

```lua
AddEventHandler("devhub_crafting:server:communityProjectCompleted", function(projectId, projectData)
    -- projectId: string - Unique ID of the completed project
    -- projectData: table - Full project configuration data
end)
```

Triggered when a community project reaches 100% completion. Use this to:

* Give rewards to players
* Send Discord announcements
* Trigger other server events
* Log completion statistics

**Example Usage:**

```lua
AddEventHandler("devhub_crafting:server:communityProjectCompleted", function(projectId, projectData)
    print("[Crafting] Community project completed: " .. projectData.label)
    
    -- Reward all online players
    local players = GetPlayers()
    for _, playerId in ipairs(players) do
        local xPlayer = ESX.GetPlayerFromId(playerId)
        if xPlayer then
            xPlayer.addMoney(5000)
            TriggerClientEvent('esx:showNotification', playerId, 
                'Community project "' .. projectData.label .. '" completed! You received $5,000!')
        end
        Wait(100)
    end
    
    -- Discord webhook notification
    PerformHttpRequest("YOUR_WEBHOOK_URL", function() end, "POST", 
        json.encode({
            content = "🎉 Community Project **" .. projectData.label .. "** has been completed!"
        }), 
        {["Content-Type"] = "application/json"}
    )
end)
```

***

### <mark style="color:yellow;">Client Function Hooks</mark>

Define these in `configs/client.lua` to hook into crafting events:

#### CanOpenCrafting

```lua
Config.OpenFunctions["CanOpenCrafting"] = function(stationId)
    -- Validate if player can open crafting
    -- Return false to prevent opening
    
    local playerPed = PlayerPedId()
    
    -- Example: Prevent opening if dead
    if IsEntityDead(playerPed) then
        return false
    end
    
    -- Example: Prevent opening in vehicle
    if IsPedInAnyVehicle(playerPed, false) then
        return false
    end
    
    -- Example: Check job requirement
    local job = ESX.GetPlayerData().job.name
    if stationId == "illegal_crafting" and job ~= "criminal" then
        return false
    end
    
    return true
end
```

#### OnCraftingOpened

```lua
Config.OpenFunctions["OnCraftingOpened"] = function(stationId)
    -- Called when crafting UI opens
    
    -- Example: Play animation
    local playerPed = PlayerPedId()
    TaskStartScenarioInPlace(playerPed, "PROP_HUMAN_BUM_BIN", 0, true)
    
    -- Example: Disable controls
    LocalPlayer.state:set('isCrafting', true, true)
end
```

#### OnCraftingClosed

```lua
Config.OpenFunctions["OnCraftingClosed"] = function(stationId)
    -- Called when crafting UI closes
    
    -- Example: Stop animation
    local playerPed = PlayerPedId()
    ClearPedTasks(playerPed)
    
    -- Example: Re-enable controls
    LocalPlayer.state:set('isCrafting', false, true)
end
```

#### OnCraftStart

```lua
Config.OpenFunctions["OnCraftStart"] = function(stationId, recipeId, amount)
    -- Called when player starts crafting
    
    -- Example: Log crafting activity
    print(string.format("Player started crafting %s x%d at %s", recipeId, amount, stationId))
    
    -- Example: Play sound effect
    PlaySoundFrontend(-1, "PICK_UP", "HUD_FRONTEND_DEFAULT_SOUNDSET", true)
end
```

***

### <mark style="color:yellow;">Server Function Hooks</mark>

Define these in `configs/server.lua` to hook into server-side events:

#### CanCraft

```lua
Config.ServerFunctions["CanCraft"] = function(source, stationId, recipeId, amount)
    -- Validate if player can craft this item
    -- Return false to prevent crafting
    
    local xPlayer = ESX.GetPlayerFromId(source)
    
    -- Example: Job requirement for certain recipes
    if recipeId:find("illegal") and xPlayer.job.name ~= "criminal" then
        TriggerClientEvent('esx:showNotification', source, "You don't have access to this recipe!")
        return false
    end
    
    -- Example: Level requirement
    local playerLevel = GetPlayerLevel(source) -- Your level system
    if recipeId:find("advanced") and playerLevel < 10 then
        TriggerClientEvent('esx:showNotification', source, "You need level 10 to craft this!")
        return false
    end
    
    return true
end
```

#### OnCraftStart

```lua
Config.ServerFunctions["OnCraftStart"] = function(source, stationId, recipeId, amount)
    -- Called when crafting begins (server-side)
    
    -- Example: Log to database
    MySQL.insert('INSERT INTO crafting_logs (player_id, recipe, amount, station, timestamp) VALUES (?, ?, ?, ?, NOW())',
        {GetPlayerIdentifier(source), recipeId, amount, stationId})
    
    -- Example: Discord logging
    local xPlayer = ESX.GetPlayerFromId(source)
    SendDiscordLog("Crafting", string.format("%s started crafting %s x%d", xPlayer.getName(), recipeId, amount))
end
```

#### OnAttachmentsApplied

```lua
Config.ServerFunctions["OnAttachmentsApplied"] = function(source, weaponModel, weaponSlot, attachmentsApplied)
    -- Called when attachments are saved to a weapon
    -- attachmentsApplied = { {category="scope", component="COMPONENT_...", label="Macro Scope"}, ... }
    
    local xPlayer = ESX.GetPlayerFromId(source)
    
    -- Example: Log attachment changes
    for _, att in ipairs(attachmentsApplied) do
        print(string.format("[Attachments] %s applied %s to %s", 
            xPlayer.getName(), att.label, weaponModel))
    end
    
    -- Example: Achievement tracking
    local totalAttachments = #attachmentsApplied
    if totalAttachments >= 5 then
        GiveAchievement(source, "weapon_customizer")
    end
end
```

#### CanUnlockBlueprint

```lua
Config.ServerFunctions["CanUnlockBlueprint"] = function(source, blueprintId)
    -- Validate if player can unlock this blueprint
    -- Return false to prevent unlocking
    
    local xPlayer = ESX.GetPlayerFromId(source)
    
    -- Example: Reputation requirement
    local rep = GetPlayerReputation(source, "crafting")
    local requiredRep = {
        ["legendary_weapon_bp"] = 1000,
        ["epic_armor_bp"] = 500,
    }
    
    if requiredRep[blueprintId] and rep < requiredRep[blueprintId] then
        TriggerClientEvent('esx:showNotification', source, 
            "You need " .. requiredRep[blueprintId] .. " crafting reputation!")
        return false
    end
    
    return true
end
```

#### OnBlueprintUnlocked

```lua
Config.ServerFunctions["OnBlueprintUnlocked"] = function(source, blueprintId)
    -- Called when a blueprint is successfully unlocked
    
    local xPlayer = ESX.GetPlayerFromId(source)
    
    -- Example: Give XP
    AddPlayerXP(source, "crafting", 100)
    
    -- Example: Discord announcement for rare blueprints
    local blueprint = Shared.Blueprints[blueprintId]
    if blueprint and blueprint.rarity == "legendary" then
        SendDiscordAnnouncement(string.format("🎉 %s unlocked the legendary blueprint: %s!", 
            xPlayer.getName(), blueprint.label))
    end
    
    -- Example: Achievement
    local unlockedCount = GetPlayerBlueprintCount(source)
    if unlockedCount >= 50 then
        GiveAchievement(source, "blueprint_collector")
    end
end
```

***

### <mark style="color:yellow;">Integration Examples</mark>

#### Close Crafting on Death (ESX)

```lua
-- client-side
AddEventHandler('esx:onPlayerDeath', function(data)
    exports['devhub_crafting']:Close()
end)
```

#### Close Crafting on Death (QBCore)

```lua
-- client-side
RegisterNetEvent('hospital:client:SetDeathState', function()
    exports['devhub_crafting']:Close()
end)
```

#### Job-Restricted Crafting Station

```lua
-- configs/client.lua
Config.OpenFunctions["CanOpenCrafting"] = function(stationId)
    if stationId == "police_armory" then
        local job = ESX.GetPlayerData().job.name
        return job == "police" or job == "sheriff"
    end
    return true
end
```

#### VIP Queue Size Bonus

```lua
-- configs/shared.lua (in crafting station config)
queueSize = {
    amount = 3,
    custom = function(source)
        local xPlayer = ESX.GetPlayerFromId(source)
        if xPlayer.getGroup() == "vip" then
            return 6  -- VIP gets 6 queue slots
        end
        return nil  -- Use default (3)
    end
}
```

#### Discord Webhook for Craft Completion

```lua
-- configs/server.lua
-- Player sets their webhook in UI settings, handled automatically

-- For server-wide logging:
Config.ServerFunctions["OnCraftStart"] = function(source, stationId, recipeId, amount)
    local xPlayer = ESX.GetPlayerFromId(source)
    PerformHttpRequest("YOUR_WEBHOOK", function() end, "POST",
        json.encode({
            embeds = {{
                title = "Craft Started",
                description = string.format("**%s** started crafting **%s** x%d", 
                    xPlayer.getName(), recipeId, amount),
                color = 3447003
            }}
        }),
        {["Content-Type"] = "application/json"}
    )
end
```
