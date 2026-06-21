# config.lua

{% code fullWidth="true" %}
```lua
Config = {}

Config.OpenGuideBook = {
    command = "guidebook",
    key = {                         -- can be nil
        defaultMapper = "KEYBOARD", -- https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/
        defaultParameter = "L"      -- https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/
    }
}

Config.SfxVolume = {
    changequestsize = 0.1, -- 0-1.0
    clickcustombutton = 0.1, -- 0-1.0
    completequest = 0.1, -- 0-1.0
    completequeststage = 0.1, -- 0-1.0
    enablequests = 0.1, -- 0-1.0
    turnpage = 0.1, -- 0-1.0
}

Config.UiScale = 1.3

Config.Book = {}

Config.Book.HomePage = {
    author = {
        title = "Author",
        name = "DEVHUB",
        img =
        "https://upload.devhub.gg/dh_upload/devhubLogo.webp",
    },
    fadingText = "GUIDEBOOKGUIDEBOOKGUIDEBOOK",
    title = "GUIDE ",
    titleExtra = "BOOK",
    subtitle = "Master your experience",
    description =
    "This comprehensive manual is designed to help you navigate and master various jobs and civilian activities available in our server. Inside, you'll find step-by-step instructions, tips, and tricks to excel in your chosen path.",
    image =
    "https://a-static.besthdwallpaper.com/grand-theft-auto-v-los-santos-sunset-panorama-wallpaper-3440x1440-19341_15.jpg",
}

Config.Book.Content = {
    {
        label = "Jobs",
        icon = "fa-solid fa-suitcase", -- https://fontawesome.com/icons
        pages = {
            title = "How to become a miner",
            description =
            "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum.", -- can be nil
            image =
            "https://wiki.highliferoleplay.net/uploads/images/gallery/2023-05/soapymining.png",                                                                                                                                                                                                                                                                                 -- can be nil                                                                                                                                                                                                                                                                        -- link to image or false
            steps = {                                                                                                                                                                                                                                                                                                                                                           -- can be nil
                {
                    title = "Step 1",
                    text =
                    "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam "
                },
                {
                    title = "Step 2",
                    text =
                    "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam "
                }
            },
            customButtons = { -- can be nil
                {
                    title = "Example Button",
                    style = "primary", -- "primary" or "secondary"
                    func = function()
                        -- your code
                        return true
                    end
                },
                {
                    title = "Example Button 2",
                    style = "secondary", -- "primary" or "secondary"
                    func = function()
                        -- your code
                        return true
                    end
                }
            },
        },
    },
}

Config.AutoEnableQuests = true -- enable quests on playerLoaded | if false you need to call server event "dh_guidebook:server:enableQuests" from client to start quests (example: TriggerServerEvent("dh_guidebook:server:enableQuests"))

Config.QuestsKeybinds = {
    changeSize = {
        defaultMapper = "KEYBOARD", -- https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/
        defaultParameter = "U"      -- https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/
    },
    cancel = {
        defaultMapper = "KEYBOARD", -- https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/
        defaultParameter = "H"      -- https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/
    },
    useHint = {
        defaultMapper = "KEYBOARD", -- https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/
        defaultParameter = "M"      -- https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/
    }
}

Config.Quests = {
    {
        title = "First Radio Connection",
        photo = "http://upload.devhub.gg/dh_upload/guidebook/radio.png",
        description = "Get a radio item and connect to any radio channel for the first time. Use /radio command or the radio item from your inventory.",
        hint = {
            coords = false,
            text = "Get a radio item and connect to any channel.",
        },
        amount = 1,
        events = { "devhub_radio:connectedToRadio" },
        rewards = {
            {
                item = "money",
                count = 1000
            }
        },
        collectedText = "Radio connections:",
        rewardText = "$1,000"
    },
}

```
{% endcode %}
