---
description: >-
  This guide covers all configurable options in the DevHub Racing script,
  organized by category and file location.
---

# ⚙️ Configuration

## <mark style="color:yellow;">Shared Configuration (configs/sh.config.lua)</mark>

### Debug and Development

```lua
Config.Debug = true
Config.DevMode = false
```

* **Debug**: Enables debug mode for troubleshooting and logging additional information.
* **DevMode**: Displays developer-related information for testing purposes.

### Skill Tree Integration

```lua
Config.SkillTreeEnabled = false
```

* **SkillTreeEnabled**: Enables skill tree integration (requires `devhub_skillTree` to be ensured first).
* Must be set to `true` and the skill tree resource must be started before this script.

### Checkpoint System

```lua
Config.EnableCheckpointMarkers = true
```

* **EnableCheckpointMarkers**: Enables/disables the 3D checkpoint marker system with UI and props.
* Disabling this will reduce resource monitor usage but remove visual markers.

### Race Blips Configuration

```lua
Config.RaceBlips = {
    enabled = true,
    showDistance = 2000.0,
    groupingDistance = 50.0,
    blipSprite = 315,
    blipColor = 46,
    blipScale = 1.0,
    blipName = "Race Start",
}
```

* **enabled**: Show/hide race start blips on the map.
* **showDistance**: Distance in meters to display race blips (default: 2000m / 2km).
* **groupingDistance**: Distance in meters to group nearby races into single blip (default: 50m).
* **blipSprite**: Blip icon sprite ID (315 = racing flag).
* **blipColor**: Blip color ID (46 = default race color).
* **blipScale**: Size of the blip icon.
* **blipName**: Display name for race start blips.

### Checkpoint Blips Configuration

```lua
Config.CheckpointBlips = {
    enabled = true,
    blipScale = 0.8,
    startSprite = 315,
    startColor = 3,
    startName = "Start",
    finishSprite = 38,
    finishColor = 1,
    finishName = "Finish",
    nextCheckpointSprite = 1,
    nextCheckpointColor = 5,
    nextCheckpointName = "Next Checkpoint",
    checkpointSprite = 1,
    checkpointColor = 4,
    checkpointName = "Checkpoint",
}
```

* **enabled**: Show/hide checkpoint blips during active races.
* **blipScale**: Default size for all checkpoint blips.
* **startSprite/Color/Name**: Settings for the start line checkpoint blip.
* **finishSprite/Color/Name**: Settings for the finish line checkpoint blip.
* **nextCheckpointSprite/Color/Name**: Settings for the next active checkpoint blip.
* **checkpointSprite/Color/Name**: Settings for regular checkpoint blips.

### MMR (Match Making Rating) System

```lua
Config.DefaultMmr = 1000
Config.AllowMrrOnAllRaces = false
Config.LeaderboardCacheTimeout = 60000 * 15
Config.MaxMmrHistory = 7
Config.MaxRaceHistory = 5

Config.MmrForWin = {
    [1] = 50,  -- 1st place
    [2] = 25,  -- 2nd place
    [3] = 10,  -- 3rd place
}

Config.MmrForLost = -25
```

* **DefaultMmr**: Starting MMR points for new players (default: 1000).
* **AllowMrrOnAllRaces**: If true, players can earn MMR on all races (not just official ones).
* **LeaderboardCacheTimeout**: Cache refresh interval in milliseconds (default: 15 minutes).
* **MaxMmrHistory**: Maximum number of MMR history entries to store per player.
* **MaxRaceHistory**: Maximum number of race history entries to display in profile.
* **MmrForWin**: MMR points awarded for top 3 positions.
* **MmrForLost**: MMR points lost per position below 3rd (multiplied by place number).

### Race Synchronization

```lua
Config.SyncPlayerPositionInterval = 1000
```

* **SyncPlayerPositionInterval**: How often to sync player positions with other racers in milliseconds (default: 1000ms / 1 second).

### Routing Bucket System

```lua
Config.DefaultRaceBucket = 2000
Config.CreatorBucket = 3000
```

* **DefaultRaceBucket**: Default routing bucket ID for races (adds server ID of first player for uniqueness).
* **CreatorBucket** :  Default routing bucket for race creator mode, to make it unique for each creator session we are adding server id of the player (adds server ID).
* Set to `false` to disable routing bucket system.
* Routing buckets isolate players in different dimensions for collision-free racing.

### Official Race Generator

```lua
Config.OfficialRaceGenerator = {
    ['Monday'] = {
        {
            start = {hour = 18, minute = 0},
            mapId = "random",
            winnerReward = 10000,
            vehicleClass = "ALL",
            collisions = true,
            fpv = false,
            laps = 3,
        },
    },
    -- ... repeated for each day
}
```

* **start**: Race start time in 24-hour format (hour and minute).
* **mapId**: Specific map ID (number) or "random" for verified maps pool.
* **winnerReward**: Cash reward for 1st place (number or `false` for no reward).
* **vehicleClass**: Allowed vehicle class ("ALL", "S", "A", "B", "C", "D").
* **collisions**: Enable/disable player vehicle collisions (props are always collision-free).
* **fpv**: Force first-person view during the race.
* **laps**: Number of laps for loop-type race maps.

{% hint style="info" %}
Set  `Config.OfficialRaceGenerator = false`  to disable automatic official race generation
{% endhint %}

### Vehicle and Traffic Settings

```lua
Config.DisableLocalVehicleDuringRace = true
```

* **DisableLocalVehicleDuringRace**: Disables all local vehicles and traffic while racing.

### Payment Configuration

```lua
Config.EntryFeePaidVia = 'cash'
Config.MoneyRewardPaidVia = 'cash'
Config.EntryFeeRewardMultiplier = 1.0
```

* **EntryFeePaidVia**: Payment method for entry fees (`'cash'` or `'bank'`).
* **MoneyRewardPaidVia**: Payment method for rewards (`'cash'` or `'bank'`).
* **EntryFeeRewardMultiplier**: Multiplier for entry fee pool rewards (1.0 = exact amount paid by all players).

### Admin Permissions

```lua
Config.IsAdminPermissionRequired = {
    ['verifyMap'] = true,
    ['createMap'] = true,
    ['createRace'] = true,
    ['editMap'] = false,
    ['editAnyMap'] = true,
}
```

* **verifyMap**: Requires admin to verify maps for official race pool.
* **createMap**: Requires admin to create new maps.
* **createRace**: Requires admin to create new races.
* **editMap**: Requires admin to edit own maps (false = anyone can edit their maps).
* **editAnyMap**: Requires admin to edit any user's maps.

### Race Music System

```lua
Config.RaceMusic = {
    enabled = true,
    volume = 0.2,
    fadeInDuration = 1000,
    fadeOutDuration = 2000,
    tracks = {
        "https://www.youtube.com/watch?v=GUgFiIvLKxk&list=...",
        "https://www.youtube.com/watch?v=8APux0hf-6M&list=...",
        -- ... more tracks
    }
}
```

* **enabled**: Enable/disable background music during races.
* **volume**: Music volume level (0.0 to 1.0).
* **fadeInDuration**: Fade in time in milliseconds when music starts.
* **fadeOutDuration**: Fade out time in milliseconds when music stops.
* **tracks**: Array of YouTube video URLs to play during races (auto-skips restricted videos).

### Teleport Settings

```lua
Config.OnRaceEndTeleportToStart = true
```

* **OnRaceEndTeleportToStart**: Teleports players back to start position when race ends.

### Laptop App Integration

```lua
Config.LaptopApp = {
    label = "Racing",
    img = "https://cfx-nui-devhub_laptop/html/images/apps/racing.png",
    path = "https://cfx-nui-devhub_racing/html/index.html",
    category = "premium",
    rating = 5,
    description = "Racing application for racing games",
    longDescription = "A competitive racing platform...",
    size = 52,
    downloads = 1000,
    reviews = { ... }
}
```

* **label**: App name displayed in laptop.
* **img**: Icon URL for the app.
* **path**: NUI path to the racing interface.
* **category**: App category in laptop store.
* **rating/description/reviews**: Metadata for laptop app display.

### Checkpoint Waypoints and Highlights

```lua
-- Waypoint and Checkpoint Highlighting
Config.NextCheckpointWaypoint = {
    enabled = true, -- Enable/disable automatic waypoint setting to next checkpoint
}

Config.NextCheckpointHighlight = {
    enabled = true, -- Enable/disable highlighting next checkpoint props with outline
    outlineColor = {r = 255, g = 255, b = 0}, -- RGB color for the outline (yellow by default)
}
```

***

## <mark style="color:yellow;">Language Configuration (configs/sh.lang.lua)</mark>

```lua
Config.Lang = {
    ['already_creating_race'] = "Already creating a race!",
    ['already_in_race'] = "You are already in a race!",
    ['must_be_in_vehicle'] = "You must be in a vehicle to create a race!",
    -- ... 100+ more translations
}
```

* **All user-facing messages**: Every notification, UI label, and system message is customizable.
* **Format placeholders**: Use `%{value}` syntax for dynamic values (e.g., `%{vehicleClass}`, `%{seconds}`).
* **Navigation labels**: Menu items like "HOME", "RACES", "MAPS", "MAP CREATOR", "PROFILE", "LEADERBOARD".
* **Race information**: Checkpoint names, race types, vehicle classes, MMR labels.
* **UI elements**: Buttons, forms, placeholders, tooltips, keyboard hints.
* **Discord webhook**: Translations for Discord announcements of official races.
* **Day names**: Monday through Sunday for official race scheduling.

***

## <mark style="color:yellow;">Skill Tree Configuration (configs/sh.skills.lua)</mark>

### XP Rewards

```lua
Config.XpForFinishingRace = {
    [1] = 100,  -- 1st place
    [2] = 50,   -- 2nd place
    [3] = 25,   -- 3rd place
    [4] = 10,   -- 4th place
    [5] = 5,    -- 5th place
    -- all other places: 0
}
```

* **XpForFinishingRace**: XP points awarded based on race position.
* Only top 5 positions receive XP.

### Skill Tree Category

```lua
Config.SkillsCategory = {
    skill = 'racing',
    title = 'Racing'
}
```

* **skill**: Internal identifier for the skill tree category.
* **title**: Display name for the skill tree tab.

### Skills List

```lua
Config.SkillsList = {
    ["racing"] = {
        {
            uid = "class_s",
            index = 26,
            title = "Class S Racing",
            icon = "fas fa-flag-checkered",
            img = "https://upload.devhub.gg/...",
            description = "Unlock Class S races...",
            points = 1,
            lines = { leftBottom = false, rightBottom = false },
        },
        -- ... more skills
    }
}
```

* **uid**: Unique identifier for the skill (referenced in code).
* **index**: Grid position in the skill tree UI.
* **title**: Display name of the skill.
* **icon**: Font Awesome icon class.
* **img**: Image URL for skill icon.
* **description**: HTML-formatted skill description.
* **points**: Skill points required to unlock.
* **effect**: Numeric value for percentage-based skills (e.g., +10% MMR).
* **lines**: Connection lines to other skills in the tree (directional: top, bottom, left, right, etc.).

### Skill Categories

* **Vehicle Class Unlocks**: `class_s`, `class_a`, `class_b`, `class_c` - Unlock participation in higher vehicle classes.
* **MMR Boosts**: `more_mmr_1`, `more_mmr_2` - Increase MMR gains by 10% per level (stacks).
* **Cash Boosts**: `more_cash_1`, `more_cash_2` - Increase cash rewards by 10% per level (stacks).

***

## <mark style="color:yellow;">Achievements Configuration (configs/sh.achivements.lua)</mark>

```lua
ACHIEVEMENTS_CONFIG = {
    ['win_1'] = { text = "Win 1 game", img = "AwardBadge.webp", index = 1 },
    ['win_10'] = { text = "Win 10 games", img = "AwardRibbon.webp", index = 2},
    ['win_25'] = { text = "Win 25 games", img = "Certificate.webp", index = 3 },
    -- ... 20 total achievements
}
```

* **text**: Achievement description displayed to players.
* **img**: Image filename for achievement icon (located in public/images/medals/).
* **index**: Display order in achievements list.

### Achievement Types

* **Win Milestones**: `win_1`, `win_10`, `win_25`, `win_50`, `win_100`, `win_200` - Win X races in 1st place.
* **Podium Finishes**: `reach_podium_10`, `reach_podium_50`, `reach_podium_100`, `reach_podium_200` - Finish top 3.
* **Race Completion**: `complete_50_races`, `complete_100_races`, `complete_250_races`, `complete_500_races` - Complete X races.
* **Class-Specific Wins**: `win_50_S_race`, `win_50_A_race`, `win_50_B_race`, `win_50_C_race`, `win_50_D_race`, `win_50_ALL_race` - Win 50 races in specific vehicle classes.

***

## <mark style="color:yellow;">Client Configuration (configs/c.functions.lua)</mark>

Open client-side functions for custom integration:

```lua
OpenClientFunctions = {
    CanJoinRace = function(raceData) return true end,
    JoinRace = function(activeRaceData) return end,
    RaceLeave = function(activeRaceData) return end,
    CheckpointPassed = function(activeRaceData, checkpointIndex) return end,
    LapComplete = function(activeRaceData, lapNumber) return end,
    RaceFinish = function(activeRaceData, position, finishData) return end,
}
```

* **CanJoinRace**: Custom validation before joining a race (return true/false).
* **JoinRace**: Triggered when player joins a race.
* **RaceLeave**: Triggered when player leaves or is removed from race.
* **CheckpointPassed**: Triggered each time a checkpoint is passed.
* **LapComplete**: Triggered after completing a lap (loop races only).
* **RaceFinish**: Triggered when player finishes the race.

***

## <mark style="color:yellow;">Server Configuration (configs/s.functions.lua)</mark>

Open server-side functions for custom integration:

```lua
OpenServerFunctions = {
    CanAddUserToRace = function(source, raceId, mapId) return true end,
    AddUserToRace = function(source, raceId, mapId) return end,
    RaceFinishByUser = function(source, raceId, position, mmrChange, moneyReward, raceTime) return end,
    RaceFinish = function(raceId, playersFinishOrder) return end,
    RaceLeave = function(source, raceId) return end,
    StartRace = function(raceId, mapId, startTime, laps, collisions, fpv, entryFee, vehicleClass, official, winnerReward) return end,
    RaceDelete = function(raceId) return end,
    CreateMap = function(mapId, name, checkpoints, decorations, raceType, isVerified, rating, start, finish, creator, creatorIdentifier) return end,
}
```

* **CanAddUserToRace**: Server-side validation before adding player to race.
* **AddUserToRace**: Triggered when player successfully joins a race.
* **RaceFinishByUser**: Triggered when individual player finishes.
* **RaceFinish**: Triggered when entire race concludes (all players finished/DNF).
* **RaceLeave**: Triggered when player leaves race.
* **StartRace**: Triggered when race countdown starts.
* **RaceDelete**: Triggered when race is deleted/cancelled.
* **CreateMap**: Triggered when new map is created.

***

## <mark style="color:yellow;">Discord Webhook Configuration (configs/s.logs.lua)</mark>

```lua
Config.LogsWebhook = "https://discord.com/api/webhooks/00000000000000"
Config.OfficialRacesAnnouncementsWebhook = "https://discord.com/api/webhooks/00000000000000"
```

* **LogsWebhook**: Discord webhook URL for general race logs (map creation, race results, etc.).
* **OfficialRacesAnnouncementsWebhook**: Discord webhook URL for official race announcements (auto-scheduled races).

***

## <mark style="color:yellow;">Vehicle Classes Configuration (configs/c.vehicleClasses.lua)</mark>

```lua
Config.VehicleClasses = {
    S = { `adder`, `banshee2`, `zentorno`, ... },
    A = { `alpha`, `elegy2`, `sultan`, ... },
    B = { `blista3`, `dominator`, `gauntlet`, ... },
    C = { `asea`, `premier`, `tailgater`, ... },
    D = { `benson`, `burrito`, `bobcatxl`, ... },
}
```

* **S Class**: Super cars (fastest, most prestigious).
* **A Class**: High-end sports cars.
* **B Class**: Mid-range sports cars and muscle cars.
* **C Class**: Regular cars and lower-end sports.
* **D Class**: Low-end vehicles, trucks, vans, utility vehicles.

### Helper Functions

```lua
GetVehicleClassByHash(vehicleHash)
IsVehicleAllowedInRace(vehicleHash, raceVehicleClass)
```

* **GetVehicleClassByHash**: Returns vehicle class (S/A/B/C/D) based on model hash.
* **IsVehicleAllowedInRace**: Checks if vehicle is allowed in a specific race class.

***

## <mark style="color:yellow;">Map Creator Props Configuration (configs/c.creatorProps.lua)</mark>

```lua
CREATOR_PROPS = {
    ['start'] = { ... },
    ['finish'] = { ... },
    ['checkpoint'] = { ... },
    ['decoration'] = { ... },
}
```

### Prop Categories

* **start**: Props available for start line (flags, gantries, signs).
* **finish**: Props available for finish line (flags, gantries).
* **checkpoint**: Props available for checkpoints (flags, poles, gantries).
* **decoration**: Props for track decoration (barrels, barriers, lights, hay bales).

### Prop Properties

```lua
{
    image = "https://upload.devhub.gg/...",
    prop = "prop_beachflag_01",
    solo = true,  -- optional
}
```

* **image**: URL to prop preview image.
* **prop**: In-game prop model name.
* **solo**: If true, only one of this prop can be placed per checkpoint (used for large gantries).

***

## Configuration Best Practices

1. **Backup before changes**: Always backup configuration files before making modifications.
2. **Test incremental changes**: Change one setting at a time and test thoroughly.
3. **Vehicle class balance**: Ensure vehicle classes are fairly distributed for competitive racing.
4. **Official race timing**: Schedule official races during peak server hours for maximum participation.
5. **MMR tuning**: Adjust MMR gains/losses based on your server's competition level.
6. **Entry fees**: Set entry fees appropriate to your server's economy.
7. **Webhook testing**: Test Discord webhooks with a test channel before using production channels.
8. **Skill tree integration**: Only enable if you have the DevHub Skill Tree resource installed.
9. **Translation completeness**: Ensure all language strings are translated for consistent user experience.
