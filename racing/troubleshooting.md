---
description: >-
  This guide helps resolve common issues with the DevHub Racing script based on
  configuration and setup.
---

# ⚠️ Troubleshooting

## <mark style="color:yellow;">Resource Not Starting</mark>

### Check Dependencies

**Problem:** Resource fails to start or shows errors in console.

**Solution:**

1.  Ensure `devhub_lib` is started **before** this resource:

    ```lua
    -- server.cfg
    ensure devhub_lib
    ensure devhub_laptop
    ensure devhub_racing
    ```
2.  If skill tree is enabled, ensure `devhub_skillTree` is started before:

    ```lua
    -- server.cfg
    ensure devhub_lib
    ensure devhub_skillTree
    ensure devhub_laptop
    ensure devhub_racing
    ```
3. Check for SQL errors - ensure database tables are created:
   * Import `sql.sql` file into your database
   * Verify tables exist: `devhub_racing_maps`, `devhub_racing_users`, `devhub_racing_races`, etc.

### Verify Config Syntax

**Problem:** Script stops loading or throws Lua errors.

**Solution:**

1. Check all config files for syntax errors:
   * Missing commas in tables
   * Mismatched brackets `{}`, `[]`
   * Unclosed strings `""`
   * Invalid Lua syntax
2. Use a Lua syntax validator or IDE with Lua support to check files.

### Check File Permissions

**Problem:** Resource cannot read configuration files.

**Solution:**

1. Ensure all files in `configs/` folder have read permissions.
2.  On Linux servers, check file ownership:

    ```bash
    chown -R fivem:fivem /path/to/devhub_racing
    chmod -R 755 /path/to/devhub_racing
    ```

***

## <mark style="color:yellow;">Players Cannot Use the System</mark>

### Check Entry Fee Requirements

**Problem:** Players cannot join races despite having money.

**Solution:**

1.  Verify payment method configuration in <mark style="color:yellow;">configs/sh.config.lua</mark>:

    ```lua
    Config.EntryFeePaidVia = 'cash'  -- or 'bank'
    ```
2. Ensure players have money in correct account:
   * `'cash'` = money on hand
   * `'bank'` = bank account balance
3. Ensure you have correctly configured [devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/ "mention")

#### Verify Vehicle Class Skills

**Problem:** Players get "must unlock class skill" error.

**Solution:**

1.  Check if skill tree is enabled in <mark style="color:yellow;">configs/sh.config.lua</mark>:

    ```lua
    Config.SkillTreeEnabled = false  -- Set to false to disable requirement
    ```
2. If skill tree enabled, ensure players have unlocked required class:
   * Class C: Starting class (should be auto-unlocked)
   * Classes B, A, S: Must be unlocked via skill tree
3. Grant skill points to players for testing:
   * Use skill tree admin commands
   * Or temporarily disable: `Config.SkillTreeEnabled = false`

#### Check Vehicle Class Restrictions

**Problem:** "Your vehicle is not allowed in this race" error.

**Solution:**

1.  Verify vehicle is in correct class in <mark style="color:yellow;">configs/c.vehicleClasses.lua</mark>:

    ```lua
    Config.VehicleClasses = {
        S = { `adder`, `zentorno`, ... },
        -- Add custom vehicles here
    }
    ```
2. Check if race has "ALL" class selected (allows any vehicle).
3. Debug vehicle hash:
   * Enable debug mode: `Config.Debug = true`
   * Check console for vehicle model hash when attempting to join
   * Add hash to appropriate vehicle class

#### Verify Coordinates and Positions

**Problem:** Players spawn in wrong locations or checkpoints don't work.

**Solution:**

1. Ensure map has valid coordinates:
   * Start position set
   * Finish position set
   * At least one checkpoint configured
2.  Check map in database for corrupted data:

    ```sql
    SELECT * FROM devhub_racing_maps WHERE id = YOUR_MAP_ID;
    ```
3. Recreate map if coordinates are invalid:
   * Delete problematic map
   * Create new map with map creator
   * Test thoroughly before making official

#### Check Admin Permissions

**Problem:** Players cannot create maps/races despite being admin.

**Solution:**

1.  Verify admin permissions in <mark style="color:yellow;">configs/sh.config.lua</mark>:

    ```lua
    Config.IsAdminPermissionRequired = {
        ['verifyMap'] = true,  -- Only admins can verify
        ['createMap'] = false,  -- Anyone can create
        ['createRace'] = false,  -- Anyone can create
        ['editMap'] = false,  -- Users can edit own maps
        ['editAnyMap'] = true,  -- Only admins edit any map
    }
    ```
2. Ensure [devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/ "mention") recognizes player as admin:
   * Check admin system in your framework
   * Test with: `Core.IsPlayerAdmin(source)` function
   * Verify ace permissions

***

## <mark style="color:yellow;">Features Not Triggering</mark>

### Verify Callback Functions

**Problem:** Custom integrations don't execute.

**Solution:**

1.  Check callback functions in <mark style="color:yellow;">configs/c.functions.lua</mark> and <mark style="color:yellow;">configs/s.functions.lua</mark>:

    ```lua
    OpenClientFunctions = {
        CanJoinRace = function(raceData)
            -- Must return true/false
            return true
        end,
        RaceFinish = function(activeRaceData, position, finishData)
            -- Your custom code here
            return  -- Must have return statement
        end,
    }
    ```
2. Ensure functions don't have errors:
   * Check F8 console for client errors
   * Check server console for server errors
   * Add debug prints to verify execution
3.  Common mistakes:

    ```lua
    -- WRONG: No return value
    CanJoinRace = function(raceData)
        print("Checking...")
        -- Missing return true
    end

    -- CORRECT:
    CanJoinRace = function(raceData)
        print("Checking...")
        return true  -- Must return boolean
    end
    ```

#### Check Event Probability Tables

**Problem:** Rewards or MMR not being awarded.

**Solution:**

1.  Verify reward tables in <mark style="color:yellow;">configs/sh.config.lua</mark>:

    ```lua
    Config.MmrForWin = {
        [1] = 50,  -- Must be > 0
        [2] = 25,
        [3] = 10,
    }
    Config.MmrForLost = -25  -- Can be negative
    ```
2.  Check if MMR is enabled for race type:

    ```lua
    Config.AllowMrrOnAllRaces = false  -- Only official races award MMR
    -- OR
    Config.AllowMrrOnAllRaces = true  -- All races award MMR
    ```
3.  Verify XP table (if skill tree enabled):

    ```lua
    Config.XpForFinishingRace = {
        [1] = 100,
        [2] = 50,
        -- ... not empty or zero
    }
    ```

#### Ensure Config Values Are Valid

**Problem:** Features behave unexpectedly or don't work.

**Solution:**

1.  Check for empty or invalid config values:

    ```lua
    -- WRONG: Empty table
    Config.MmrForWin = {}

    -- WRONG: Nil value
    Config.DefaultMmr = nil

    -- CORRECT:
    Config.MmrForWin = { [1] = 50, [2] = 25, [3] = 10 }
    Config.DefaultMmr = 1000
    ```
2. Verify data types:
   * Numbers: `1000`, `50.5`, `-25`
   * Strings: `"cash"`, `"ALL"`
   * Booleans: `true`, `false`
   * Tables: `{}`, `{ key = value }`
3.  Check for typos in config keys:

    ```lua
    -- WRONG: Typo in property name
    Config.DefautMmr = 1000  -- Should be "DefaultMmr"

    -- CORRECT:
    Config.DefaultMmr = 1000
    ```

***

## <mark style="color:yellow;">Props, UI and Events Not Working</mark>

### Verify Prop Models Exist

**Problem:** Props don't appear in map creator or during races.

**Solution:**

1.  Check prop model names in <mark style="color:yellow;">configs/c.creatorProps.lua</mark>:

    ```lua
    {
        image = "https://...",
        prop = "prop_beachflag_01",  -- Must be valid GTA V prop model
    }
    ```
2. Test prop models in-game:
   * Create spawn prop command example:`/spawnprop prop_beachflag_01`&#x20;
   * Verify model exists in game files
3. Common issues:
   * Typo in prop name
   * DLC prop requiring specific game build
   * Custom prop not installed on server
   * Model streaming issues (add to `fxmanifest.lua` if needed)

#### Enable Debug Mode

**Problem:** Unable to identify what's failing.

**Solution:**

1.  Enable debug mode in <mark style="color:yellow;">configs/sh.config.lua</mark>:

    ```lua
    Config.Debug = true
    Config.DevMode = true  -- Additional developer information
    ```
2. Check logs in:
   * F8 console (client-side)
   * Server console (server-side)
   * Browser console (NUI errors)
3. Look for specific error messages:
   * Script errors (Lua syntax)
   * Network errors (API calls failing)
   * Asset loading errors (props, images, sounds)

#### Verify Asset References

**Problem:** Images, sounds, or UI elements don't load.

**Solution:**

1.  Check image URLs in configs:

    ```lua
    Config.LaptopApp = {
        img = "https://cfx-nui-devhub_laptop/html/images/apps/racing.png",
        -- URL must be accessible
    }
    ```
2. Verify sound files exist:
   * Check `public/sounds/` folder
   * Files: `countdown.wav`, `fireworks.mp3`, `swoosh.wav`
   * Ensure proper file formats (WAV, MP3, OGG)
3.  Check NUI resource path:

    ```lua
    path = "https://cfx-nui-devhub_racing/html/index.html",
    -- Must match resource name exactly
    ```
4. Test asset loading:
   * Open browser console (NUI devtools)
   * Check for 404 errors or loading failures

#### Restart in Correct Order

**Problem:** Script partially works or has intermittent issues.

**Solution:**

1.  **Proper startup order:**

    ```lua
    -- server.cfg (correct order)
    ensure devhub_lib
    ensure devhub_skillTree  # if skill tree enabled
    ensure devhub_laptop
    ensure devhub_racing
    ```
2.  **Server restart sequence:**

    ```
    ensure devhub_lib
    ensure devhub_skillTree
    ensure devhub_laptop
    ensure devhub_racing
    ```
3.  **Individual resource restart:**

    ```
    refresh  # Refresh resource cache
    ensure devhub_racing  # Start resource
    ```
4. **Full server restart** recommended after:
   * Database changes
   * Major configuration changes
   * Adding/removing dependencies

***

## <mark style="color:yellow;">Race Mechanics Issues</mark>

### Checkpoint Not Detecting

**Problem:** Players drive through checkpoints but they don't register.

**Solution:**

1.  Check checkpoint marker settings and devMode:

    ```lua
    Config.EnableCheckpointMarkers = true  -- Must be enabled
    Config.DevMode = true
    ```
2. Verify checkpoint placement in map:
   * Checkpoints too far apart
   * Checkpoint trigger zone too small
   * Checkpoints placed incorrectly (underground, in air)
3. Test with debug mode enabled:
   * See checkpoint coordinates
   * Verify player position vs checkpoint position
   * Check trigger distance calculations
4. Recreate problematic checkpoints:
   * Edit map in map creator
   * Delete and re-place checkpoint

#### Race Will Not Start

**Problem:** Countdown doesn't begin or race is stuck.

**Solution:**

1. Check minimum player requirement:
   * At least 1 player required
   * Verify players are in waiting room state
2.  Verify start time configuration:

    ```lua
    Config.OfficialRaceGenerator = {
        ['Monday'] = {
            {
                start = {hour = 18, minute = 0},  -- Must be valid time
                -- ...
            }
        }
    }
    ```
3. Check server time vs configured time:
   * Server might be in different timezone
   * Use `/time` command to check current server time
   * Adjust scheduled times accordingly
4. Manual race start issues:
   * Check if start delay expired
   * Verify race is not already in progress
   * Ensure no conflicting race instances

#### Blips Not Showing

**Problem:** Map markers and checkpoint blips invisible.

**Solution:**

1.  Check blip configuration in <mark style="color:yellow;">configs/sh.config.lua</mark>:

    ```lua
    Config.RaceBlips = {
        enabled = true,  -- Must be true
        showDistance = 2000.0,  -- Increase if needed
        -- ...
    }

    Config.CheckpointBlips = {
        enabled = true,  -- Must be true
        -- ...
    }
    ```
2. Verify player distance to race start:
   * Must be within `showDistance` range
   *   Increase range for testing:

       ```lua
       showDistance = 10000.0,  -- 10km
       ```
3. Check for blip sprite conflicts:
   * Try different blip sprite IDs
   * Use unique blip sprites not used elsewhere

#### Collisions Not Working as Expected

**Problem:** Player collisions disabled when they should be enabled (or vice versa).

**Solution:**

1.  Check race configuration:

    ```lua
    collisions = true,  -- Player collisions enabled
    collisions = false,  -- Player collisions disabled (ghost mode)
    ```
2. Note: **Props are always collision-free** by design:
   * Prevents players getting stuck on decorations
   * Allows passing through checkpoint markers
   * Cannot be changed via config
3. Verify entity collision settings:
   * Check if other scripts override collision
   * Test in isolated environment (no other scripts)

***

## <mark style="color:yellow;">UI and Display Issues</mark>

### Racing App Not Opening

**Problem:** Laptop app doesn't display racing interface.

**Solution:**

1.  Check laptop integration:

    ```lua
    Config.LaptopApp = {
        path = "https://cfx-nui-devhub_racing/html/index.html",
        -- Must match resource name
    }
    ```
2. Test NUI directly:
   * F8 console: `loadnui https://cfx-nui-devhub_racing/html/index.html`
   * Check NUI console for errors
3. Ensure laptop resource is compatible:
   * Verify `devhub_laptop` is installed (if required)
   * Check laptop app registration

#### Translations Not Displaying

**Problem:** UI shows translation keys instead of text.

**Solution:**

1.  Check language file completeness in <mark style="color:yellow;">configs/sh.lang.lua</mark>:

    ```lua
    Config.Lang = {
        ['already_in_race'] = "You are already in a race!",
        -- All keys must have values
    }
    ```
2. Verify translation key matches code:
   * Check for typos in translation keys
   * Ensure key exists in config
   * Use exact key name (case-sensitive)
3. Missing translations:
   * Copy missing keys from default `sh.lang.lua`
   * Translate to your language
   * Restart resource

#### Music Not Playing

**Problem:** Race music doesn't play or is silent.

**Solution:**

1.  Check music configuration in <mark style="color:yellow;">configs/sh.config.lua</mark>:

    ```lua
    Config.RaceMusic = {
        enabled = true,  -- Must be true
        volume = 0.2,  -- Increase if too quiet
        tracks = {
            "https://www.youtube.com/watch?v=...",
            -- Must be valid, embeddable YouTube links
        }
    }
    ```
2. YouTube embed restrictions:
   * Some videos are blocked by copyright
   * Use royalty-free music or licensed tracks
   * System auto-skips blocked videos to next track
3. Player settings:
   * Press `G` in-game to toggle music
   * Check game audio settings (not muted)
   * Test with different browsers (NUI)

***

## <mark style="color:yellow;">Discord Integration Issues</mark>

### Webhooks Not Sending

**Problem:** Discord messages not appearing.

**Solution:**

1.  Verify webhook URLs in <mark style="color:yellow;">configs/s.logs.lua</mark>:

    ```lua
    Config.LogsWebhook = "https://discord.com/api/webhooks/1234567890/ABCDEF..."
    Config.OfficialRacesAnnouncementsWebhook = "https://discord.com/api/webhooks/..."
    ```
2. Test webhook URLs:
   * Paste in browser (should show "Invalid webhook token" - this is correct)
   * Copy from Discord server settings > Integrations > Webhooks
   * Ensure webhook is not deleted in Discord
3. Check Discord channel permissions:
   * Webhook must have permission to send messages
   * Channel must exist and be accessible
   * Webhook not rate-limited

#### Announcement Formatting Issues

**Problem:** Discord messages display incorrectly.

**Solution:**

1.  Check translation strings for webhook in <mark style="color:yellow;">configs/sh.lang.lua</mark>:

    ```lua
    Config.Lang = {
        ['webhook_title'] = "🏁 New Official Race Scheduled 🏁",
        ['webhook_description'] = "A new official race has been generated!",
        -- ... all webhook_ keys must exist
    }
    ```
2. Verify embed structure in code:
   * Discord embeds have field limits (256 chars for field names, 1024 for values)
   * Check for special characters breaking JSON
   * Validate embed colors (must be valid hex or decimal)

***

## <mark style="color:yellow;">Database Issues</mark>

### Data Not Saving

**Problem:** Player stats, maps, or races not persisting.

**Solution:**

1.  Verify SQL tables exist:

    ```sql
    SHOW TABLES LIKE 'devhub_racing%';
    ```
2. Import SQL file if missing:
   * Locate `sql.sql` in resource folder
   * Restart server after import
3. Check database permissions:
   * User must have INSERT, UPDATE, DELETE permissions
   * Verify connection in [devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/ "mention") config
   * Test with simple query
4. Look for SQL errors in console:
   * Check server console for MySQL errors
   * Common: Duplicate keys, constraint violations
   * Fix data inconsistencies in database

#### Leaderboard Not Updating

**Problem:** Leaderboard shows old data or doesn't refresh.

**Solution:**

1.  Check cache timeout:

    ```lua
    Config.LeaderboardCacheTimeout = 60000 * 15  -- 15 minutes
    ```
2. Force cache refresh:
   * Wait for cache timeout to expire
   * Restart resource: `restart devhub_racing`
3. Verify leaderboard query:
   * Check for database errors in console
   * Test query directly in database
   * Ensure proper indexing on database tables

***

## <mark style="color:yellow;">General Troubleshooting Checklist</mark>

Before reporting issues, verify:

* [ ] All dependencies are installed and started (devhub\_lib, devhub\_laptop)
* [ ] Database tables are created from `sql.sql`
* [ ] Config files have no syntax errors
* [ ] Server and resource are fully restarted
* [ ] Debug mode is enabled to see error messages
* [ ] Console (F8 and server) checked for errors
* [ ] Your framework is working correctly and is configured in [devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/ "mention")
* [ ] No conflicting resources are running
* [ ] Game cache is cleared (if using custom assets)
* [ ] Server artifacts are up to date

**If issues persist:**

1. Enable debug mode for detailed logs
2. Check client (F8) , server and NUI console
3. Test with minimal configuration (disable features one by one)
4. Verify issue on clean server with only required resources
5. Check for updates to `devhub_racing` , `devhub_lib, devhub_laptop`&#x20;
6. Contact DevHub support with:
   * Server console logs
   * Client console logs (F8)
   * NUI console logs (F8 NUI devtools)
   * Config files (with sensitive data removed \[webhooks])
   * Steps to reproduce the issue
