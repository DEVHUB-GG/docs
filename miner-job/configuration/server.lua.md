---
description: Documentation for server.lua Configuration
---

# server.lua

## <mark style="color:yellow;">**Drill Item Configuration**</mark>

```lua
Config.MenuItem = "dh_drill"
```

* **Description**: Specifies the item required to initiate the drilling process.
* **Example**: `"dh_drill"` means players need this item to start drilling.

{% hint style="danger" %}
Dont forget to add the item <mark style="color:blue;">**dh\_drill**</mark> to the item list. An example of how to add to \
<mark style="color:purple;">qb-inventory</mark> and <mark style="color:purple;">ox-inventory</mark> can be found in the <mark style="color:red;">items.lua</mark> file
{% endhint %}

***

## <mark style="color:yellow;">**Ore Deletion Chance**</mark>

```lua
Config.DeleteOreChance = 10
```

* **Description**: Sets the probability (in percentage) that an ore will be removed after a successful drill.
* **Example**: `10` means there is a 10% chance of the ore disappearing post-drilling.

***

## <mark style="color:yellow;">**Money Type for Jobs and Quests**</mark>

```lua
Config.MoneyType = "cash"
```

* **Description**: Determines whether the player receives job payment as cash or bank balance.
* **Options**: `"cash"` or `"bank"`

```lua
Config.MoneyTypeQuest = "cash"
```

* **Description**: Specifies the method of payment (cash or bank) for completing quests.
* **Options**: `"cash"` or `"bank"`

***

## <mark style="color:yellow;">**Webhook Configuration**</mark>

```lua
Config.Webhook = "https://discord.com/api/webhooks/890000000000000000/"
```

* **Description**: URL for sending logs or notifications to a Discord channel.
* **Note**: Replace with your webhook URL for proper functionality.
