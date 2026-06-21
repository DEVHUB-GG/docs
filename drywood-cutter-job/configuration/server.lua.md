---
description: Documentation for server.lua Configuration
---

# server.lua

## <mark style="color:yellow;">Item Reward</mark>

```lua
Config.ItemReward = {
    {
        name = "wood",
        amount = 1,
    },
}
```

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
