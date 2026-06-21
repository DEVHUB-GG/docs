---
description: >-
  This guide explains how the DevHub Laptop system works from initial boot to
  application usage.
---

# 🌊 Script Flow

## <mark style="color:yellow;">First Boot Phase</mark>

### Player Opens Laptop

1. Player executes a command or uses an item to open the laptop
2. If first boot detected, system begins setup

### Loading Sequence

The laptop displays a loading screen with multiple simulated initialization steps:

### User Profile Configuration

Player configures their laptop profile:

1. **Username Selection**:
   * Player enters desired username
2. **Avatar Selection**:
   * Player chooses from available avatars
   * Avatars defined in `Config.DefaultAvatars`
3. **Wallpaper Selection**:
   * Player chooses from available wallpapers
   * Wallpapers defined in `Config.DefaultWallpapers`

### Security Settings

Player configures device security:

1. **Password Protection** (optional):
   * Player enters password
   * Player confirms password
   * System validates passwords match
   * **Password is not encrypted**
2. **Face ID Setup** (optional):
   * Player can enable Face ID authentication
   * Enables quick unlock without password
3. **No protection, user will be automatilcy loged in**

***

## <mark style="color:yellow;">Home Screen Operations</mark>

### Desktop Layout

The home screen displays:

* **Wallpaper**: Active background image
* **App Icons**: Grid of installed applications
* **Taskbar**: Bottom bar with quick access tools
* **System Tray**: Settings, WiFi, battery, time indicators
* **Notification Area**: System and app notifications

### App Management

#### Opening Apps

1. Player double clicks app icon on desktop

#### App Window Controls

Each app window has controls:

* **Minimize**: Hides window, keeps app running in background
* **Maximize**: Expands window to full screen (if supported)
* **Close**: Exits app and closes window

#### Multiple Apps

Players can:

* Run multiple apps simultaneously
* Switch between apps using taskbar
* Minimize apps to taskbar
* Close individual apps

### System Settings Access

Player can open settings panel to:

1. Toggle WiFi on/off
2. Toggle Bluetooth on/off
3. Enable/disable sound effects
4. Enable/disable ECO mode
5. Adjust screen brightness
6. Adjust system volume
7. Change wallpaper
8. Update security settings
9. Edit username and avatar

### Power Options

From taskbar menu, player can:

* **Shut Down**: Closes laptop, saves state, returns to game

***

## <mark style="color:yellow;">App Store Flow</mark>

### Browsing Apps

1. Player opens App Store from desktop
2. App Store loads available apps from `Config.Apps`
3. Apps displayed by category
4. Recommended apps shown at top (if configured)

### App Categories

Apps are organized into categories:

* **System**: Core system applications
* **Premium**: Premium/paid applications
* **Other**: Additional applications
* Custom categories defined in `Config.AppCategories`

### Searching Apps

1. Player enters search term
2. System filters apps by name and description
3. Results update in real-time
4. Player can clear search to show all apps

### Viewing App Details

1. Player clicks on an app card
2. App details page opens showing:
   * App name and icon
   * Rating and author
   * File size and download count
   * Short and long descriptions
   * Gallery screenshots
   * User reviews
   * Install/Uninstall button

### Installing Apps

#### Regular Apps

1. Player clicks "Download" button
2. System checks if app is already installed
3. App added to user's installed apps
4. App icon appears on desktop
5. Success notification displays

#### Premium Apps

1. Player clicks on premium app
2. Premium content message displays
3. System checks if server has premium access
4. If no access, player cannot install
5. Message directs player to contact server owner

### Uninstalling Apps

1. Player opens App Store
2. Player navigates to installed app
3. Player clicks "Uninstall" button
4. System checks `dontAllowUninstall` property
5. If allowed, app removed from desktop

***

## <mark style="color:yellow;">Individual App Flows</mark>

### Calculator

1. Player opens Calculator app
2. Calculator interface displays
3. Player clicks number and operator buttons
4. Expression builds in display
5. Player clicks equals to calculate result
6. Result displays or error shown if invalid

### Notepad

1. Player opens Notepad app
2. Notepad loads saved notes from database
3. Player can create new note
4. Player enters note title and content
5. Player clicks Save button
6. Note stored in database
7. Player can select, edit, or delete existing notes

### CMD (Command Line)

1. Player opens CMD app
2. Terminal displays welcome message
3. Player types commands and presses Enter
4. System processes command
5. Output displays in terminal
6. Available commands:
   * `help`: Show available commands
   * `clear`: Clear terminal screen
   * `whoami`: Display current username
   * `date`: Show current date and time

### Browser

1. Player opens Browser app
2. Browser opens in full-screen mode
3. Home page displays with quick links
4. Player can:
   * Enter URL in address bar
   * Click quick link buttons
   * Navigate to websites
   * Use iframe to display web content
5. Browser loads external websites
6. Player can return home or enter new URL

### Clock

1. Player opens Clock app
2. Clock displays:
   * Current local time
   * Current day of week
   * World clocks from `Config.WorldClocks`
3. Each world clock shows:
   * Location name
   * Current time in that timezone
   * Time difference from local time
