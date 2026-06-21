---
description: >-
  This guide helps resolve common issues with the DevHub Laptop script based on
  configuration and setup.
---

# ⚠️ Troubleshooting

## <mark style="color:yellow;">First Steps:</mark>

1. Ensure you have devhub\_lib installed and configured.
2. Make a fresh resintall of a script and devhub\_lib
3. Make sure you havent changed resource name
4. Make full server restart

## <mark style="color:yellow;">Resource Not Starting</mark>

### Check FiveM Manifest

**Problem:** Resource fails to start or shows errors in console.

**Solution:**

1.  Ensure the resource name in `server.cfg` matches the folder name:

    ```lua
    ensure devhub_laptop
    ```
2. Check that all required files are present:
   * `fxmanifest.lua`
   * `configs/` folder with all config files
   * `escrowed/` folder with Lua scripts
   * `html/` folder with compiled NUI files

### Check File Permissions

**Problem:** Resource fails to load files or access data.

**Solution:**

1. Ensure the resource folder has proper read permissions
2.  On Linux, verify ownership and permissions:

    ```bash
    chown -R fivem:fivem /path/to/devhub_laptop
    chmod -R 755 /path/to/devhub_laptop
    ```
3. Check that escrowed files are not corrupted

***

## <mark style="color:yellow;">Database Issues</mark>

### SQL Tables Not Created

**Problem:** Players cannot save data, getting database errors.

**Solution:**

1. Import the `sql.sql` file into your database
2.  Verify tables exist by running:

    ```sql
    SHOW TABLES LIKE 'devhub_laptop%';
    ```
3. Expected tables should include:
   * User profile tables
   * App data tables
   * Crime Traders tables (if using premium app)
4. If tables don't exist, manually execute the SQL file

### Images or Assets Not Loading

**Problem:** UI loads but images are missing or broken.

**Solution:**

1.  Verify image paths in config files use correct format:

    ```lua
    -- Correct format for local resources:
    "https://cfx-nui-devhub_laptop/html/images/apps/calculator.png"
    ```
2. Check that files exist in `html/images/` folder
3. Verify file extensions match exactly (case-sensitive)
4. Clear FiveM cache and restart

***

## <mark style="color:yellow;">Configuration Errors</mark>

### Lua Syntax Errors

**Problem:** Script fails to start with Lua syntax errors in console.

**Solution:**

1. Check all config files for syntax errors:
   * Missing commas in tables
   * Mismatched brackets `{}`, `[]`
   * Unclosed strings `""`
2.  Common mistakes:

    ```lua
    -- WRONG: Missing comma
    Config.Apps = {
        ['calculator'] = { label = "Calculator" }
        ['notepad'] = { label = "Notepad" }
    }

    -- CORRECT:
    Config.Apps = {
        ['calculator'] = { label = "Calculator" },
        ['notepad'] = { label = "Notepad" },
    }
    ```
3. Use a Lua validator or IDE with syntax checking
4. Check for invalid characters or encoding issues

***

## <mark style="color:yellow;">Development Issues</mark>

### Configuration Not Taking Effect

**Problem:** Changes to config files don't appear in-game.

**Solution:**

1. Ensure you saved the config file after editing
2.  Restart the resource:

    ```
    restart devhub_laptop
    ```
3. If still not working, restart the entire server
4. Check server console for Lua syntax errors
5. Verify you edited the correct config file in `configs/` folder

***

## <mark style="color:yellow;">Advanced Troubleshooting</mark>

### Enable Debug Mode

To get detailed error information:

1.  Enable debug mode in config:

    ```lua
    Config.Debug = true
    ```
2. Check server console for debug messages
3. Open browser console NUI errors
4. Monitor network tab for failed requests

### Check Resource Order

Ensure proper resource start order in `server.cfg`:

```lua
ensure devhub_lib              # Required dependency
ensure devhub_laptop           # This resource
```

### Clear Cache

If experiencing persistent issues:

1. Clear FiveM cache:
   * Close FiveM
   * Delete cache folder in FiveM application data
   * Restart FiveM
2. Restart server and resource

### Database Verification

Check database integrity:

```sql
-- Verify tables exist
SHOW TABLES LIKE 'devhub_laptop%';

-- Check for corrupt data
SELECT * FROM devhub_laptop_users WHERE data IS NULL;

-- Verify relationships
SELECT COUNT(*) FROM devhub_laptop_notes;
```

***

## <mark style="color:yellow;">Getting Help</mark>

### Before Requesting Support

1. Check this troubleshooting guide thoroughly
2. Enable debug mode in `configs/sh.config.lua` and collect error messages
3. Verify all configuration files in `configs/` folder are correct
4. Check database tables exist and have data
5. Try on a clean script install
6. Confirm you haven't modified any files outside the `configs/` folder

### Information to Provide

When requesting support, include:

1. FiveM server version
2. Framework name and version
3. Full error messages from console
4. Browser console errors
5. Configuration files (if modified)
6. Steps to reproduce the issue
7. What you've already tried

***

**Remember**: Most issues are caused by configuration errors or missing dependencies. Always verify your setup matches the requirements before assuming a script bug.
