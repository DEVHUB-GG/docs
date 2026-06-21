# config.lua

{% code fullWidth="true" %}
```lua
Config = {}
Config.PauseMenuData = {} -- Do not change this

Config.Debug = false -- Enable debug prints, this will help you find issues

Config.SfxVolume = { -- Sound effects volume
    click = 0.5, -- 0 - 1.0
}

-- Gta color for menu borders and highlights
Config.GtaColor = 12 -- https://docs.fivem.net/natives/?_0x2ACCB195F3CCD9DE

Config.PauseMenuData.News = { -- News messages to show in the pause menu. Can use html formatting
    "We are launching a 15% discount on all our products starting today and running until Sunday, November 30",
    "We have something special for you – to celebrate 2,000 members on Discord, enjoy 20% OFF everything in our Tebex store!",
    "To celebrate Independence Day, we are offering a 15% discount on individual products, available until July 10th, 2025."
}

Config.PauseMenuData.Tasks = { -- Define tasks for players to complete
    {
        description = "Use radio 1 time", -- task description
        maxStep = 1, -- number of steps to complete the task
        stepLabel = "", -- Text that will be displayed near current step and max step (ex. for "x" value: 10/20x)
        rewards = false, -- items that will be given after complete task, can be false
        money = 500, -- how much money player will get after complete task, can be false
        handler = function() end, -- custom server function that will be executed after complete task, can be false
        customCompleteText = false, -- Text displayed after a task is completed (instead of the default).
        rewardText = "500$", -- Text displayed in UI as reward
        events = { "devhub_radio:connectedToRadio" }, -- set client events here that, when triggered, will complete the step.
    },
    {
        description = "Drive 50km in any vehicle",
        maxStep = 50,
        stepLabel = " km",
        rewards = {
            { name = "water", count = 5 }
        },
        money = 300,
        handler = function() end,
        customCompleteText = "Road warrior!",
        rewardText = "5x Water",
        events = { "vehicle:distance" },
    },
    {
        description = "Complete 5 deliveries",
        maxStep = 5,
        stepLabel = "",
        rewards = {
            { name = "delivery_box", count = 1 }
        },
        money = 700,
        handler = function() end,
        customCompleteText = "Delivery master!",
        rewardText = "1x Delivery Box",
        events = { "job:deliveryComplete" },
    },
    {
        description = "Win 3 races",
        maxStep = 3,
        stepLabel = "",
        rewards = {
            { name = "race_ticket", count = 2 }
        },
        money = 1000,
        handler = function() end,
        customCompleteText = "Champion!",
        rewardText = "2x Race Ticket",
        events = { "race:win" },
    },
}

Config.PauseMenuData.KeyboardRows = { -- How keyboard will be displayed in pause menu
    {"ESC", "1", "2", "3", "4", "5", "6", "7", "8", "9", "0"},
    {"TAB", "Q", "W", "E", "R", "T", "Y", "U", "I", "O", "P"},
    {"CAPS", "A", "S", "D", "F", "G", "H", "J", "K", "L", ";"},
    {"SHIFT_L", "Z", "X", "C", "V", "B", "N", "M", ",", ".", "/"},
    {"CTRL_L", "ALT_L", "SPACE", "ALT_R", "CTRL_R"},
}

Config.PauseMenuData.Keybinds = { -- Keybinds displayed on keyboard in pause menu
    -- id - Key ID from KeyboardRows
    -- action - Action displayed in pause menu (to show more then one use {"some action"})
    -- category - Key category displayed in pause menu
    { id = "W", action = { "Move Forward", "Other action" }, category = "Movement" },
    { id = "A", action = "Move Left", category = "Movement" },
    { id = "S", action = "Move Backward", category = "Movement" },
    { id = "D", action = "Move Right", category = "Movement" },
    { id = "E", action = "Interact", category = "General" },
    { id = "F", action = "Primary Action / Horn", category = "General" },
    { id = "R", action = "Reload Weapon", category = "Combat" },
    { id = "SPACE", action = "Jump / Handbrake", category = "Movement" },
    { id = "TAB", action = "Open Inventory", category = "Interface" },
    { id = "M", action = "Open Map", category = "Interface" },
    { id = "ESC", action = "Open Menu / Close UI", category = "Interface" },
    { id = "SHIFT_L", action = "Sprint / Nitro", category = "Movement" },
    { id = "CTRL_L", action = "Crouch / Duck", category = "Movement" },
    { id = "1", action = "Select Item/Weapon 1", category = "Hotbar" },
    { id = "2", action = "Select Item/Weapon 2", category = "Hotbar" },
    { id = "T", action = "Open Chat", category = "Communication" },
    { id = "G", action = "Vehicle Options", category = "Vehicle" },
}

Config.PauseMenuData.Changelogs = { -- Changelogs to show in the pause menu. You can use html formatting
    {
        title = "1.0.0 (December 29, 2025)",
        changes = {
            {
                title = "Initial Release",
                changes = {
                    "Pause menu system introduced with keyboard, keybinds, and changelogs.",
                    "Added player tasks and rewards system.",
                    "Integrated server and vehicle rules display.",
                },
            },
            {
                title = "Improvements",
                changes = {
                    "Optimized UI performance and responsiveness.",
                    "Improved translations and multi-language support.",
                },
            },
        }
    },
    {
        title = "1.0.1 (January 10, 2026)",
        changes = {
            {
                title = "Bug Fixes",
                changes = {
                    "Fixed issue with task progress not saving correctly.",
                    "Resolved minor UI glitches in pause menu.",
                },
            },
            {
                title = "New Features",
                changes = {
                    "Added new hot deal box with Discord link.",
                    "Expanded rules and changelogs sections.",
                },
            },
        }
    }
}

Config.PauseMenuData.Rules = { -- Rules to show in the pause menu. You can use html formatting
    {
        title = "Vehicle Rules",
        rules = {
            "Do not intentionally ram other players' vehicles.",
            "Obey traffic signals and speed limits in city zones.",
            "No reckless driving in populated areas.",
            "Park vehicles only in designated areas.",
            "Report abandoned or glitched vehicles to staff.",
        }
    },
    {
        title = "Server Rules",
        rules = {
            "Respect all players and staff members.",
            "No cheating, exploiting, or use of third-party software.",
            "Roleplay is mandatory in all public areas.",
            "Do not spam chat or use offensive language.",
            "Follow staff instructions at all times.",
        }
    },
    {
        title = "Combat Rules",
        rules = {
            "No random deathmatching (RDM).",
            "No combat logging or disconnecting to avoid roleplay.",
            "Use weapons responsibly and only in appropriate scenarios.",
            "Do not abuse game mechanics for unfair advantage.",
        }
    },
    {
        title = "Economy Rules",
        rules = {
            "No real-world trading of in-game items or currency.",
            "Do not exploit bugs for financial gain.",
            "All trades must be fair and consensual.",
        }
    }
}

-- You can replace hot deal image in html/images/hot_deal.webp
Config.PauseMenuData.HotDeal = { -- Hot Deal box options.
    action = "link", -- "link" / "export"
    data = "https://discord.com/invite/8uBVD36ZxD", -- export format resourceName.exportName
    closeAfterClick = true
}

Config.TasksResetAfter = 72 -- The number of hours after which task progress will be cleared (time is counted individually for each player from the moment they joined server for the first time).

```
{% endcode %}
