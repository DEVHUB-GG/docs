# ❓ Multiple recepies configuration

## 🧪 How to Create Custom Recipes

Learn how to create and configure custom recipes using the new `Shared.MultipleRecipes` system. This flexible system allows you to create multiple unique recipes for a single crafting table without using the visual table generator.

### 🤔 Why Use Custom Recipes?

The `Shared.MultipleRecipes` system offers several advantages over the traditional table generator:

#### **Benefits:**

* **Multiple Recipes Per Table**: Add 10, 20, or even more different recipes to a single crafting table
* **No Visual Configuration Required**: Skip the complex prop positioning and animation setup
* **Easy Management**: Add, remove, or modify recipes with simple configuration changes
* **Mix & Match**: Combine different recipe types at the same table
* **Quick Setup**: Create recipes in minutes instead of hours
* **Reusable Tables**: Use existing table designs with new recipes

#### **Perfect For:**

* Alchemy stations with multiple potion types
* Cooking tables with various food recipes
* Weapon crafting benches with different weapons
* Any scenario where you need recipe variety without visual complexity

***

{% stepper %}
{% step %}
### Understanding the Structure

The custom recipe system consists of two main parts:

#### **1. Recipe Definition** (`configs/shared.lua`)

```lua
Shared.MultipleRecipes = {
    ["your_recipe_uid"] = {
        itemsNeeded = { -- Items required to craft
            { name = "item_name", amount = 2 },
            { name = "another_item", amount = 1 },
        },
        rewards = { -- Items given after successful craft
            { name = "crafted_item", amount = 1 },
        },
        tableUid = "herbal_table", -- Visual table reference
    },
}
```

#### **2. Table Assignment** (`configs/server.lua`)

```lua
Config.Tables = {
    {
        coords = vec4(x, y, z, heading),
        recipeUids = { "your_recipe_uid", "another_recipe" }, -- Assign recipes to table
        snapToGround = true,
    },
}
```
{% endstep %}

{% step %}
### Creating Your First Custom Recipe

Let's create a simple health potion recipe:

#### **Step 1: Add Recipe to `configs/shared.lua`**

Open `configs/shared.lua` and find or create the `Shared.MultipleRecipes` section:

```lua
Shared.MultipleRecipes = {
    ["health_potion_basic"] = {
        itemsNeeded = {
            {
                name = "red_herb",
                amount = 3,
            },
            {
                name = "water_bottle", 
                amount = 1,
            },
        },
        rewards = {
            {
                name = "health_potion",
                amount = 1,
            },
        },
        tableUid = "herbal_table", -- This must match a key in Shared.Recepie
    },
}
```

#### **Step 2: Configure Table in `configs/server.lua`**

Add or modify a table to use your new recipe:

```lua
Config.Tables = {
    {
        coords = vec4(250.0, -750.0, 34.0, 90.0), -- Your table position
        recipeUids = { "health_potion_basic" }, -- Reference your recipe
        snapToGround = true,
    },
}
```
{% endstep %}

{% step %}
### Adding Multiple Recipes to One Table

Let's create a complete alchemy station with multiple potion types:

#### **Multiple Recipe Example:**

```lua
Shared.MultipleRecipes = {
    -- Health Potions
    ["health_potion_small"] = {
        itemsNeeded = {
            { name = "red_herb", amount = 2 },
            { name = "water_bottle", amount = 1 },
        },
        rewards = {
            { name = "health_potion_small", amount = 1 },
        },
        tableUid = "herbal_table",
    },
    ["health_potion_large"] = {
        itemsNeeded = {
            { name = "red_herb", amount = 5 },
            { name = "crystal_shard", amount = 1 },
            { name = "water_bottle", amount = 1 },
        },
        rewards = {
            { name = "health_potion_large", amount = 1 },
        },
        tableUid = "herbal_table",
    },
    
    -- Mana Potions
    ["mana_potion_small"] = {
        itemsNeeded = {
            { name = "blue_herb", amount = 2 },
            { name = "water_bottle", amount = 1 },
        },
        rewards = {
            { name = "mana_potion_small", amount = 1 },
        },
        tableUid = "herbal_table",
    },
    
    -- Special Potions
    ["invisibility_potion"] = {
        itemsNeeded = {
            { name = "shadow_essence", amount = 1 },
            { name = "blue_herb", amount = 3 },
            { name = "rare_mushroom", amount = 1 },
            { name = "water_bottle", amount = 1 },
        },
        rewards = {
            { name = "invisibility_potion", amount = 1 },
        },
        tableUid = "herbal_table",
    },
}
```

#### **Assign All Recipes to One Table:**

```lua
Config.Tables = {
    {
        coords = vec4(250.0, -750.0, 34.0, 90.0),
        recipeUids = { 
            "health_potion_small",
            "health_potion_large", 
            "mana_potion_small",
            "invisibility_potion"
        },
        snapToGround = true,
    },
}
```
{% endstep %}

{% step %}
### Recipe Configuration Options

#### **Required Fields:**

| Field         | Description             | Example                           |
| ------------- | ----------------------- | --------------------------------- |
| `itemsNeeded` | Array of required items | `{ name = "herb", amount = 2 }`   |
| `rewards`     | Array of items given    | `{ name = "potion", amount = 1 }` |
| `tableUid`    | Visual table reference  | `"herbal_table"`                  |

#### **Item Structure:**

```lua
{
    name = "item_identifier",  -- Must match your inventory system
    amount = 5,               -- Quantity required/given
}
```

#### **Available Table UIDs created by us:**

Use these `tableUid` values to reference existing table visuals:

* `"herbal_table"` - Alchemy table with cauldron
* `"spices"` - Simple crafting table
{% endstep %}

{% step %}
### Advanced Recipe Examples

#### **Cooking Station Example:**

```lua
-- Cooking Recipes
["grilled_meat"] = {
    itemsNeeded = {
        { name = "raw_meat", amount = 1 },
        { name = "salt", amount = 1 },
    },
    rewards = {
        { name = "grilled_meat", amount = 1 },
    },
    tableUid = "cooking_station",
},

["meat_stew"] = {
    itemsNeeded = {
        { name = "raw_meat", amount = 2 },
        { name = "potato", amount = 3 },
        { name = "carrot", amount = 2 },
        { name = "water_bottle", amount = 1 },
    },
    rewards = {
        { name = "meat_stew", amount = 1 },
    },
    tableUid = "cooking_station",
},
```

#### **Weapon Crafting Example:**

```lua
-- Weapon Recipes
["iron_sword"] = {
    itemsNeeded = {
        { name = "iron_ingot", amount = 3 },
        { name = "wood_handle", amount = 1 },
    },
    rewards = {
        { name = "iron_sword", amount = 1 },
    },
    tableUid = "smithing_table",
},

["steel_armor"] = {
    itemsNeeded = {
        { name = "steel_ingot", amount = 8 },
        { name = "leather", amount = 4 },
        { name = "cloth", amount = 2 },
    },
    rewards = {
        { name = "steel_chestplate", amount = 1 },
    },
    tableUid = "smithing_table",
},
```
{% endstep %}

{% step %}
### Testing Your Recipes

#### **1. Restart the Script**

After adding your recipes, restart the resource:

```
restart your_resource_name
```

#### **2. Test In-Game**

1. Go to your configured table location
2. Ensure you have the required items in inventory
3. Interact with the table
4. Your custom recipes should appear in the crafting menu

#### **3. Troubleshooting**

**Recipe Not Showing:**

* Check that `recipeUids` in server config matches your recipe names exactly
* Verify the table coordinates are correct
* Ensure `tableUid` references an existing table in `Shared.Recepie`

**Items Not Working:**

* Confirm item names match your inventory system exactly
* Check item amounts are correct
* Verify players have the required items
{% endstep %}

{% step %}
### Best Practices

#### **Recipe Organization:**

```lua
Shared.MultipleRecipes = {
    -- Group similar recipes together
    
    -- === HEALTH POTIONS ===
    ["health_small"] = { ... },
    ["health_medium"] = { ... },
    ["health_large"] = { ... },
    
    -- === MANA POTIONS ===
    ["mana_small"] = { ... },
    ["mana_medium"] = { ... },
    
    -- === FOOD RECIPES ===
    ["bread"] = { ... },
    ["soup"] = { ... },
}
```

#### **Naming Convention:**

* Use descriptive, unique recipe UIDs
* Include category/type in the name: `"weapon_iron_sword"`, `"potion_health_large"`
* Avoid spaces, use underscores: `"my_recipe"` not `"my recipe"`

#### **Recipe Balance:**

* Start with simple recipes (2-3 ingredients)
* Gradually add complexity for rare items
* Consider ingredient availability in your server economy
* Test crafting times and player experience
{% endstep %}
{% endstepper %}

***

### 🎯 Quick Start Template

Copy this template to get started quickly:

```lua
-- In configs/shared.lua
Shared.MultipleRecipes = {
    ["your_recipe_name"] = {
        itemsNeeded = {
            { name = "ingredient_1", amount = 1 },
            { name = "ingredient_2", amount = 2 },
        },
        rewards = {
            { name = "crafted_item", amount = 1 },
        },
        tableUid = "herbal_table", -- Change to your preferred table
    },
}

-- In configs/server.lua  
Config.Tables = {
    {
        coords = vec4(0.0, 0.0, 0.0, 0.0), -- Change to your coordinates
        recipeUids = { "your_recipe_name" },
        snapToGround = true,
    },
}
```

Replace the placeholders with your actual values and you're ready to go!

***

### 📝 Summary

The custom recipe system gives you the power to create rich, varied crafting experiences without the complexity of visual table generation. Use it to:

* Quickly add multiple recipes to existing tables
* Create themed crafting stations (alchemy, cooking, smithing)
* Experiment with different ingredient combinations
* Scale your crafting system as your server grows

Start simple with a few recipes and expand as you become comfortable with the system. Happy crafting! 🎉
