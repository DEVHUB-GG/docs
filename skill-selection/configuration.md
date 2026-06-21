# 🛠️ Configuration

## <mark style="color:yellow;">Main Configuration (sh.main.lua)</mark>

### Basic Settings

```lua
Config.MenuCommand = "skillSelection"  -- Command to open menu (string or false)
Config.Keybind = "F6"                 -- Keybind to open menu (string or false)
```

***

### Skill Tree Settings

```lua
Config.UseDataFromSkillTree = false  -- true/false
```

Use data about unlocked skills from the skill tree

{% hint style="info" %}
[BUY SKILL TREE](https://store.devhub.gg/product/6440792-1)
{% endhint %}

{% content-ref url="skill-tree-connection.md" %}
[skill-tree-connection.md](skill-tree-connection.md)
{% endcontent-ref %}

***

### Abilities

<pre class="language-lua"><code class="lang-lua">Config.Abilities = {
    ['wizard'] = {
        name = "Wizard",
        displayOrder = 1,
<strong>        isVisible = function(source)
</strong>            -- return true if the ability should be visible in the UI
            -- server side
            return true
        end,
        abilities = {
            ['water_spell'] = {
                label = "Water spell",
                icon = "fas fa-user",
                useCooldown = 5,
                onUse = function()
                    -- client side
                    print("Water spell used by "..GetPlayerName(PlayerId()))
                    return true -- return true if the ability was used successfully, false cooldown will not be triggered
                end
            },
            ['fire_spell'] = {
                label = "Fire spell",
                icon = "fire_spell.png",
                useCooldown = 5,
                onUse = function()
                    -- client side
                    print("Fire spell used by "..GetPlayerName(PlayerId()))
                    return true -- return true if the ability was used successfully, false cooldown w
                end
            },
        }
    },
    ['vampire'] = {
        name = "Vampire",
        displayOrder = 2,
        isVisible = function(source)
            -- return true if the ability should be visible in the UI
            -- server side
            return true
        end,
        abilities = {
            ['more_hp'] = {
                label = "More Hp",
                icon = "more_hp.png",
                useCooldown = 5,
                onUse = function()
                    -- client side
                    print("More hp used by "..GetPlayerName(PlayerId()))
                    return true -- return true if the ability was used successfully, false cooldown w
                end
            },
        }
    },
}
</code></pre>

***

### Hotbar settings

```lua
Config.UseBuildInHotBar = true   --- true/false
```

Use the build in hotbar, if set to false you can use your own hotba if you are using custom hotbar use client event to listen for hotbar changes&#x20;

```lua
RegisterNetEvent('devhub_skillSelection:client:syncSlots', function(data) end) 
```

data is a table with structure like this:&#x20;

{% code overflow="wrap" fullWidth="false" %}
```lua
{
   [2] = { -- slot 2
         category = "wizard", 
         uid = "water_spell", 
         label = "Water spell", 
         icon = "fas fa-user"
   }, 
   [6] = { -- slot 6
         category = "vampire", 
         uid = "vampire_spell", 
         label = "Vampire spell", 
         icon = "fas fa-user
   },  
}
```
{% endcode %}

***

```lua
Config.DisplayHotBarOnScriptLoad = true   --- true/false
```

Display the hotbar on script load, if set to false you can set when hotbar should be displayed using client event:

```lua
TriggerEvent('devhub_skillSelection:client:hotBarDisplayStatus', true)
```

***

{% hint style="warning" %}
If you are using custom hotbar you also need to set the hotbar slots.
{% endhint %}

```lua
Config.HotBarSlots = {
    { slotKey = "1", slotLabel = "1" },
    { slotKey = "2", slotLabel = "2" },
    { slotKey = "3", slotLabel = "3" },
    { slotKey = "4", slotLabel = "4" },
    { slotKey = "5", slotLabel = "5" },
    { slotKey = "6", slotLabel = "6" },
    { slotKey = "7", slotLabel = "7" },
    { slotKey = "8", slotLabel = "8" },
    { slotKey = "K", slotLabel = "K" },
}
```

slotKey - used in key mapping\
slotLabel - used in Ui

***

### CustomHotBarCss

Adjust the positioning of hotbar.

```lua
Config.CustomHotBarCss = {
    left = "50%",
    top = "95%",
    scale = 1.0,
    gap = "0.75vw",
    reverseRow = false,
    column = false,
}
```

***

## <mark style="color:yellow;">Language Configuration (sh.lang.lua)</mark>

The language file contains all text strings used in the UI. Each string can be customized:

```lua
Config.Lang = {
    ['skill_select'] = "Skill select",
    ['skill_select_hotbar'] = "Skill select hotbar",
    ['confirm'] = "Confirm",
}
```

***

## <mark style="color:yellow;">Framework File (s.framework.lua)</mark>

Set up your framework here for use within the isVisible function in Config.Abilities

***

## <mark style="color:yellow;">Functions File (c.functions.lua)</mark>

Here, you can customize the behavior of the **useRaycast** function. Whole **useRaycast** function behaviour is <mark style="color:green;">open source</mark>.

### **useRaycast** usage:

```lua
local maxDistance = 50
local coords, entity = useRaycast(maxDistance)
if not coords then return false end
```

Input:

If the `coords` returns false, it indicates that the user attempted to point at coordinates too far from their original location.
