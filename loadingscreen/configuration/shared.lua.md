---
description: Documentation for shared.lua Configuration
---

# shared.lua

```lua
Config = {}

-- How to use local images files:
-- 1. Place your files in the html folder of your resource.
-- 2. Base it on the example nui://devhub_loadingscreen/html/image.webp

Config.Logo = "nui://devhub_loadingscreen/html/logo.webp" -- Path to the logo image, can be a URL or a local file path (html folder)

Config.ShowLogoInTheCenter = false -- If true, logo will be displayed in the center of the screen, otherwise in the center you will have your game is loading text.

Config.Name = "SERVER NAME" -- Name of your server, will be displayed in the top left corner


Config.Background = {
    type = "images", -- "images" or "video"
    volume = 0.05, -- Default volume of the background sound or video (0.0 to 1.0)
    -- if type is "images"
    images = { -- Path to the logo image, can be a URL or a local file path (html folder)
        "nui://devhub_loadingscreen/html/background_1.webp",
        "nui://devhub_loadingscreen/html/background_2.webp",
        "nui://devhub_loadingscreen/html/background_3.webp",
    },
    imageChangeInterval = 10000, -- 10 seconds
    randomizeImages = true, -- If true, starting image will be random, otherwise it will start from the first image
    sound = 'nui://devhub_loadingscreen/html/music.ogg', -- Set to false if you don't want to play sound in the background. Hosted URL or Local File
    -- Examples for sound:
    -- Local file: 'music.ogg'   [RECOMMENDED]
    -- Hosted URL: 'https://example.com/music.mp3'  
    -- if type is "video"
    video = "nui://devhub_loadingscreen/html/video.mp4", -- Hosted URL or local video file
    
    -- Examples for video:
    -- Local video file: 'nui://devhub_loadingscreen/html/video.mp4'
    -- Hosted URL: 'https://example.com/video.mp4'

    -- YOUTUBE VIDEOS ARE NOT SUPPORTED ANYMORE
}

Config.Lang = {
    ['your_game_is'] = "YOUR GAME IS",
    ['loading'] = "LOADING..."
}

Config.Socials = {
    {
        icon = "fab fa-youtube",
        link = "https://www.youtube.com/@devhub-fivem",
    },
    {
        icon = "fab fa-discord",
        link = "https://discord.gg/JNrJTgybvQ",
    },
    {
        icon = "fab fa-instagram",
        link = "https://instagram.com/yourprofile",
    },
    {
        icon = "fab fa-tiktok",
        link = "https://tiktok.com/yourprofile",
    },
}

Config.Popups = {
    interval = 5000, -- 5 seconds
    data = {
        {
            title = "Ben Ten",
            text = "This is a review message with some info about your server.",
            rating = 5, -- if you add rating, it will show stars instead of tip
        },
        {
            title = "How dose tips work ?",
            text = "This is a tip message with some info about your server.",
        },
    }
}

-- If you want to disable some page in the menu, remove it from the list
Config.Pages = {
    { name = "News", value = "news", icon = "far fa-newspaper" },
    { name = "Updates", value = "updates", icon = "far fa-bell" },
    { name = "Featured Packs", value = "packs", icon = "far fa-star" },
    { name = "Keybinds", value = "keybinds", icon = "far fa-key" },
    { name = "Team", value = "team", icon = "far fa-users" },
}

-- You can also make it into partners showcase, or your team members.
Config.Team = {
    {
        name = "John Doe",
        role = "Lead Developer",
        image = "https://randomuser.me/api/portraits/men/1.jpg",
    },
    {
        name = "Jane Smith",
        role = "UI/UX Designer",
        image = "https://randomuser.me/api/portraits/women/76.jpg",
    },
    {
        name = "Mike Johnson",
        role = "Backend Developer",
        image = "https://randomuser.me/api/portraits/men/72.jpg",
    },
}

-- Can be used to showcase featured packs from your store, scripts, in game offers.
Config.FeaturedPacks = {
    {
        name = "Skill Tree v2",
        image = "https://upload.devhub.gg/dh_upload/website/skillTree/skillTreeThumbnail.webp",
        link = "https://store.devhub.gg/product/6440792-1",
        bestseller = true,
    },
    {
        name = "Multiplayer Pool Cleaner Job",
        image = "https://dunb17ur4ymx4.cloudfront.net/packages/images/27bb8e4d54c0d787a887cb69f750d07c4c06e524.webp",
        link = "https://store.devhub.gg/product/6803226",
    },
    {
        name = "3D Crafting Table",
        image = "https://dunb17ur4ymx4.cloudfront.net/packages/images/70d918a953795f91ea11c930a91b446f71bb0bb8.png",
        link = "https://store.devhub.gg/product/6719154",
    },
    {
        name = "Gym Script",
        image = "https://dunb17ur4ymx4.cloudfront.net/packages/images/e2f904ac118a640bf20a18ab4b6a7d234c1b53db.webp",
        link = "https://store.devhub.gg/product/6789437",
    },

}

Config.KeyboardRows = {
    {"ESC", "1", "2", "3", "4", "5", "6", "7", "8", "9", "0"},
    {"TAB", "Q", "W", "E", "R", "T", "Y", "U", "I", "O", "P"},
    {"CAPS", "A", "S", "D", "F", "G", "H", "J", "K", "L", ";"},
    {"SHIFT_L", "Z", "X", "C", "V", "B", "N", "M", ",", ".", "/"},
    {"CTRL_L", "ALT_L", "SPACE", "ALT_R", "CTRL_R"},
}

Config.Keybinds = {
    { id = "W", display = "W", action = "Move Forward", category = "Movement" },
    { id = "A", display = "A", action = "Move Left", category = "Movement" },
    { id = "S", display = "S", action = "Move Backward", category = "Movement" },
    { id = "D", display = "D", action = "Move Right", category = "Movement" },
    { id = "E", display = "E", action = "Interact", category = "General" },
    { id = "F", display = "F", action = "Primary Action / Horn", category = "General" },
    { id = "R", display = "R", action = "Reload Weapon", category = "Combat" },
    { id = "SPACE", display = "Spacebar", action = "Jump / Handbrake", category = "Movement" },
    { id = "TAB", display = "Tab", action = "Open Inventory", category = "Interface" },
    { id = "M", display = "M", action = "Open Map", category = "Interface" },
    { id = "ESC", display = "Escape", action = "Open Menu / Close UI", category = "Interface" },
    { id = "SHIFT_L", display = "Left Shift", action = "Sprint / Nitro", category = "Movement" },
    { id = "CTRL_L", display = "Left Ctrl", action = "Crouch / Duck", category = "Movement" },
    { id = "1", display = "1", action = "Select Item/Weapon 1", category = "Hotbar" },
    { id = "2", display = "2", action = "Select Item/Weapon 2", category = "Hotbar" },
    { id = "T", display = "T", action = "Open Chat", category = "Communication" },
    { id = "G", display = "G", action = "Vehicle Options", category = "Vehicle" },
}

Config.News = {
    {
        date = "2025-10-01",
        title = "New Feature Release",
        content = "We are excited to announce the release of our new feature that enhances user experience and performance. We are excited to announce the release of our new feature that enhances user experience and performance.",
        image = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRW9h4fU35Nb2lGTCcT-GS1pVkWd5gqV0yegw&s",
    },
    {
        date = "2025-09-01",
        title = "Performance Enhancements",
        content = "We have made several performance enhancements to improve the overall user experience.",
        image = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRW9h4fU35Nb2lGTCcT-GS1pVkWd5gqV0yegw&s",
    }
}

Config.Updates = {
    {
        date = "2025-10-01",
        title = "New Feature Release",
        changes = {"Added support for new themes.", "Improved performance of the application.", "Fixed bugs related to user authentication."},
    },
    {
        date = "2025-09-15",
        title = "Bug Fixes and Improvements",
        changes = {"Fixed an issue with the login process.", "Improved the loading speed of the application.", "Updated the user interface for better usability."},
    },
}

-- You can hide the default busy spinner in the bottom right corner of the screen by adding the following command to your server.cfg:
-- setr sv_showBusySpinnerOnLoadingScreen false
```
