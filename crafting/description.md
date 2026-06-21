---
description: >-
  This document describes all configuration options for the DevHub Truck robbery
  resource.
---

# 📜 Description

## Overview

DevHub Crafting is an advanced, feature-rich crafting system designed for FiveM servers. It provides a complete crafting experience with a modern, animated UI built with Vue.js, supporting multiple crafting stations, blueprints, weapon attachments, community-driven projects, and detailed player statistics.

## Key Features

* **Multi-Station Crafting System** - Create multiple crafting workbenches with unique recipes, props, and camera angles
* **Blueprint Collection** - Unlockable blueprints with rarity tiers and custom unlock conditions
* **Weapon Attachment System** - Full 3D preview with real-time attachment management (suppressors, scopes, grips, magazines, tints)
* **Community Projects** - Server-wide collaborative goals where players contribute materials
* **Statistics Tracking** - Comprehensive player statistics including crafted items, time spent, and leaderboards
* **Admin Panel** - Full in-game configuration editor for categories, blueprints, crafts, and workbenches. Open it with the `/craftingadmin` command (admin only).
* **Queue System** - Time-based crafting with queue management and reordering
* **Discord Integration** - Personal webhook notifications when crafts complete

## Supported Frameworks

* **ESX**
* **QBCore**
* **QBOX**
* **VRP**
* **Custom frameworks** via devhub\_lib bridge

## Supported Inventories

* ox\_inventory
* qb-inventory
* qs-inventory
* esx\_inventoryhud
* and more...
* Custom inventories via devhub\_lib

***

## UI Features

### Modern Interface

* Elegant dark theme with gold accents
* Smooth page transitions and entrance animations
* Three-panel layout (item list, 3D preview, queue/details)
* Responsive design with custom scrollbars

### Crafting Panel

* Category filtering with custom icons
* List and grid view modes
* Multiple sort options (Name, Craftable, Blueprint status)
* Real-time ingredient tracking with visual indicators
* 3D prop preview with mouse rotation

### Weapon Attachments Panel

* Real-time 3D weapon preview
* Mouse drag rotation and scroll zoom
* Dynamic bone-based attachment positioning
* Visual connection lines from UI to attachment points
* Install/remove attachments with inventory integration

### Blueprint Collection

* Visual grid display with rarity colors
* Lock/unlock status indicators
* Sorting by rarity, name, or unlock status
* Detailed tooltip with required materials

### Statistics Dashboard

* Top 5 most crafted items display
* Total crafting time tracking
* Resources used counter
* Blueprint collection progress
* Community donation totals

***

## 🛡️ Admin Panel — `/craftingadmin` Command

The Admin Panel is one of the most powerful features of DevHub Crafting. It gives administrators a full, real-time in-game interface for configuring every aspect of the crafting system — no file editing required.

### Command

```
/craftingadmin
```

### Requirements

{% hint style="info" %}
This command requires **admin permissions** only. No additional configuration is needed — just make sure you are logged in as an admin on your server.
{% endhint %}

### How to Open the Admin Panel

1. Join the server with an **admin account**
2. Open the chat and type `/craftingadmin`
3. The Admin Panel will open immediately in-game

### What Can You Configure

The Admin Panel is divided into four main tabs, each offering full CRUD (create, read, update, delete) capabilities:

#### 📂 Categories Tab

Manage the category list used to filter recipes in the crafting UI.

* Add new categories with a custom label and [FontAwesome](https://fontawesome.com/icons) icon
* Edit existing category names, icons, and display order
* Remove unused categories
* Reorder categories via the `order` field (lower number = shown first)

#### 📘 Blueprints Tab

Control which blueprints exist and how they can be unlocked.

* Create blueprints with name, description, image URL, and rarity tier
* Set required materials for each blueprint
* Toggle the `hidden` flag — hidden blueprints show as `???` until unlocked
* Define custom unlock conditions (e.g. mission completion, reputation threshold)

#### ⚒️ Crafts / Recipes Tab

Define every craftable item and its recipe.

* Add recipes with ingredients (item name + amount), output item, and crafting time
* Set `maxQuantity` to limit how many can be queued at once (`-1` = unlimited)
* Link a recipe to a blueprint requirement
* Assign a category and rarity for visual styling
* Attach a 3D prop model for the in-game preview

#### 🏭 Crafting Stations Tab

Place and configure crafting workbenches in the world.

* Set world coordinates (`vector4` — includes heading) directly in-game
* Choose the 3D prop model displayed at the station
* Configure the camera position and lookAt point for the UI camera
* Set the default and VIP queue sizes
* Enable/disable the Weapon Attachments tab per station
* Add an optional map blip (name, sprite, size, colour)

Players outside these groups will receive an access-denied notification when attempting to run the command.

{% hint style="info" %}
See the [Admin System documentation](../scripts/devhub_lib/admin.md) to adjust which permission groups are treated as admins for your framework.
{% endhint %}

### Best Practices

* After making changes in the panel, **export your configuration** to the relevant `configs/` files so they persist after a resource restart
* Test each new recipe/station **on a development server** before pushing to production
