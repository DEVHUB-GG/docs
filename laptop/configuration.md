---
description: This guide documents all configuration files for the DevHub Laptop system.
---

# ⚙️ Configuration

## <mark style="color:yellow;">sh.config.lua</mark>

Main shared configuration file for the laptop system. This file controls debug settings, default assets, pre-installed apps, and browser configuration.

### Debug Settings

```lua
Config.Debug = true
```

* `Config.Debug` (boolean): Enables debug mode with console logging
  * `true` = Show debug messages for troubleshooting
  * `false` = Disable debug output (recommended for production)

### Default Wallpapers

```lua
Config.DefaultWallpapers = {
    "https://cfx-nui-devhub_laptop/html/images/wallpapers/wallpaper_1.webp",
    "https://cfx-nui-devhub_laptop/html/images/wallpapers/wallpaper_2.webp",
    "https://cfx-nui-devhub_laptop/html/images/wallpapers/wallpaper_3.webp",
}
```

* `Config.DefaultWallpapers` (table): List of wallpaper URLs available to users
  * First wallpaper is active by default
  * First wallpaper cannot be removed by the user
  * Add or remove wallpaper URLs to customize available options
  * Use either CDN URLs or local paths with `cfx-nui-devhub_laptop` prefix

### Default Avatars

```lua
Config.DefaultAvatars = {
    "https://cfx-nui-devhub_laptop/html/images/avatars/avatar_1.webp",
    "https://cfx-nui-devhub_laptop/html/images/avatars/avatar_2.webp",
    "https://cfx-nui-devhub_laptop/html/images/avatars/avatar_3.webp",
    "https://cfx-nui-devhub_laptop/html/images/avatars/avatar_4.webp",
}
```

* `Config.DefaultAvatars` (table): List of avatar image URLs for user profiles
  * Add or remove avatar URLs to customize available options
  * Use either CDN URLs or local paths with `cfx-nui-devhub_laptop` prefix

### Default Settings

```lua
Config.DefaultSettings = {
    wifi = true,
    bluetooth = false,
    soundless = false,
    ecoMode = false,
    brightness = 100,
    volume = 20,
}
```

* `wifi` (boolean): WiFi connection status on first boot
  * `true` = WiFi enabled
  * `false` = WiFi disabled
* `bluetooth` (boolean): Bluetooth connection status on first boot
  * `true` = Bluetooth enabled
  * `false` = Bluetooth disabled
* `soundless` (boolean): Sound mode on first boot
  * `true` = Silent mode enabled (no sounds)
  * `false` = Sounds enabled
* `ecoMode` (boolean): ECO Mode (battery saver) on first boot
  * `true` = ECO Mode enabled
  * `false` = ECO Mode disabled
* `brightness` (number): Screen brightness level (0-100)
  * Default: `100` (full brightness)
* `volume` (number): System volume level (0-100)
  * Default: `20`

### Pre-Installed Apps

```lua
Config.PreInstalledApps = {
    'notepad',
    'calculator',
    'cmd',
    'browser',
    'appStore',
    'clock',
}
```

* `Config.PreInstalledApps` (table): Apps that come pre-installed on every new laptop
  * App IDs must match those defined in `sh.apps.lua`
  * Users cannot uninstall apps with `dontAllowUninstall = true`
  * Add or remove app IDs to customize the default app selection

### World Clocks

```lua
Config.WorldClocks = {
    {name = "Warsaw (CET)", timezone = "Europe/Warsaw"},
    {name = "New York (EST)", timezone = "America/New_York"},
    {name = "Tokyo (JST)", timezone = "Asia/Tokyo"},
    {name = "London (GMT)", timezone = "Europe/London"},
}
```

* `Config.WorldClocks` (table): List of world clocks displayed in the Clock app
  * `name` (string): Display name for the time zone
  * `timezone` (string): IANA timezone identifier
  * Add or remove entries to customize available world clocks
  * Use standard IANA timezone database names

### Browser Quick Links

```lua
Config.BrowserQuickLinks = {
    {title = "Google", url = "https://www.google.com/search?igu=1", icon = "fab fa-google"},
    {title = "CodeSandbox", url = "https://codesandbox.io/embed/new?codemirror=1", icon = "fas fa-code"},
    {title = "Devhub Store", url = "https://store.devhub.gg", icon = "fas fa-laptop-code"},
    {title = "Wikipedia", url = "https://wikipedia.org", icon = "fas fa-book"},
}
```

*   `Config.BrowserQuickLinks` (table): Quick access links shown on browser home page

    * `title` (string): Display name for the quick link
    * `url` (string): Target URL to open when clicked
    * `icon` (string): Font Awesome icon class for the link button
    * Add or remove entries to customize browser quick links
    * Use Font Awesome 5 icon classes for icons

    _Keep in mind some websites might not work_

***

## <mark style="color:yellow;">sh.apps.lua</mark>

Configuration file for apps, app categories, and the app store. This file defines all available applications, their properties, and store settings.

### Recommended Apps

```lua
Config.RecommendedApps = {
    -- {
    --     appId = 'racing',
    --     bannerImage = 'https://upload.devhub.gg/dh_upload/laptop/appStore/racingBannerLaptop2.webp',
    -- },
}
```

*   `Config.RecommendedApps` (table): Featured apps displayed prominently in the App Store

    * `appId` (string): App ID that matches an entry in `Config.Apps`
    * `bannerImage` (string): URL to banner image for the featured app
    * Leave empty to disable recommended apps section
    * Add entries to promote specific apps

    Recommended Apps are beaing loaded from out external source, but by editing this config you can add your own apps.

### App Categories

```lua
Config.AppCategories = {
    ['system'] = {
        label = "System",
        order = 1,
    },
    ['premium'] = {
        label = "Premium",
        order = 99,
    },
    ['other'] = {
        label = "Other", 
        order = 100,
    },
}
```

* `Config.AppCategories` (table): Categories for organizing apps in the App Store
  * `label` (string): Display name for the category
  * `order` (number): Sort order (lower numbers appear first)
  * **Required categories**: `system`, `premium`, `other` (script will add them if removed)
  * Add custom categories with unique keys and labels

### Apps Configuration

Each app in `Config.Apps` follows this structure:

#### System App Example (CMD)

```lua
['cmd'] = {
    label = "CMD",
    img = "https://cfx-nui-devhub_laptop/html/images/apps/cmd.png",
    path = "app_cmd",
    category = "system",
    rating = 4,
    description = "Command Line Interface",
    longDescription = "A command line tool for executing system commands...",
    author = "DEVHUB",
    size = 122,
    downloads = 1000,
    class = 'regular',
    galleryImages = {
        'https://cfx-nui-devhub_laptop/html/images/appGallery/cmd.webp',
    },
    reviews = {
        {
            user = "TechUser123",
            rating = 5,
            comment = "Perfect command line tool!",
            date = "2025-08-10"
        },
    },
}
```

### App Properties Reference

#### Required Properties

* `label` (string): Display name of the app
* `img` (string): URL to app icon image
* `path` (string): Component path identifier (must match Vue component)
* `category` (string): Category key from `Config.AppCategories`
* `rating` (number): App rating (1-5)
* `description` (string): Short description shown in app list
* `longDescription` (string): Detailed description shown in app details
* `author` (string): Developer name
* `size` (number): File size in MB
* `downloads` (number): Download count displayed in store
* `class` (string): App type - `'regular'` or `'premium'`

#### Optional Properties

* `fullScreen` (boolean): Whether app opens in full-screen mode
  * Default: `false`
  * Example: Browser app uses `fullScreen = true`
* `dontAllowUninstall` (boolean): Prevents users from uninstalling the app
  * Default: `false`
  * Example: App Store has `dontAllowUninstall = true`
* `developerLogo` (string): URL to developer logo image
  * Shown in app details page
  * Optional, typically used for premium apps
* `galleryImages` (table): Array of screenshot URLs
  * Shown in app details page
  * Can be empty table
* `reviews` (table): Array of user review objects
  * Each review contains: `user`, `rating`, `comment`, `date`
  * Can be empty table

### Gallery Images Structure

```lua
galleryImages = {
    'https://cfx-nui-devhub_laptop/html/images/appGallery/browser_1.webp',
    'https://cfx-nui-devhub_laptop/html/images/appGallery/browser_2.webp',
}
```

### Reviews Structure

```lua
reviews = {
    {
        user = "WebSurfer",
        rating = 5,
        comment = "Excellent browser! Fast loading and great security features.",
        date = "2025-08-13"
    },
    {
        user = "InternetUser",
        rating = 5,
        comment = "Very reliable and user-friendly.",
        date = "2025-08-11"
    },
}
```

* `user` (string): Reviewer username
* `rating` (number): Review rating (1-5)
* `comment` (string): Review text
* `date` (string): Review date in YYYY-MM-DD format

***

## <mark style="color:yellow;">sh.crimeTraders.lua</mark>

Configuration file for the Crime Traders premium app. This file controls traders, shop items, XP requirements, and crime actions.

### Traders Configuration

```lua
Config.CrimeTradersTraders = { 
    ['trader_1'] = {
        name = 'Marcus',
        image = './images/avatars/trader_1.webp',
        order = 1,
        shopItems = {
            ['1'] = {
                {
                    name = 'weapon_pistol',
                    price = 150,
                    amount = 1
                },
            },
            ['5'] = {
                {
                    name = 'weapon_assaultrifle',
                    price = 300,
                    amount = 1
                },
            },
        }
    },
}
```

#### Trader Properties

* `name` (string): Display name of the trader
* `image` (string): Path to trader avatar image
* `order` (number): Sort order for trader display
* `shopItems` (table): Items available at each level

#### Shop Items Structure

Shop items are organized by level requirement:

```lua
shopItems = {
    ['1'] = {  -- Level 1 items
        {
            name = 'weapon_pistol',
            price = 150,
            amount = 1
        },
    },
    ['5'] = {  -- Level 5 items
        {
            name = 'weapon_assaultrifle',
            price = 300,
            amount = 1
        },
    },
}
```

* Level keys (string): Player level required to unlock items
* `name` (string): Item spawn name or `'money'` for cash
* `price` (number): Cost in Crime Points (CP)
* `amount` (number): Quantity given (or money amount if name is `'money'`)

#### Special Item: Money

```lua
{
    name = 'money',
    price = 300,
    amount = 5000
}
```

To give money instead of items, set `name = 'money'` and `amount` to the cash value. (**It will use Core.AddCash**)

### XP Requirements

```lua
Config.XpRequirements = {
    [1] = 0,
    [2] = 100,
    [3] = 250,
    [4] = 500,
    [5] = 1000,
    [6] = 2000,
    [7] = 3500,
    [8] = 5500,
    [9] = 8000,
    [10] = 12000,
    [11] = 16000,
    [12] = 21000,
    [13] = 27000,
    [14] = 35000,
    [15] = 45000,
}
```

* `Config.XpRequirements` (table): XP needed to reach each trader level
  * Key: Level number
  * Value: Total XP required to reach that level
  * Players level up when they reach the XP threshold
  * Adjust values to make leveling easier or harder

### Crime Actions

```lua
Config.CrimeActions = {}
```

* `Config.CrimeActions` (table): Defines available crime activities
  * Empty by default
  * Crime actions are loaded from our external source or a script you own
  * Contain crime job definitions and rewards

***

## <mark style="color:yellow;">sh.lang.lua</mark>

Language and translation configuration file. Contains all UI text strings used throughout the laptop system.

### Language Structure

All translations are stored in the `Config.Lang` table:

```lua
Config.Lang = {
    ['key'] = "Translated text",
}
```
