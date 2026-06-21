# ❔ FAQ

## ❔ FAQ

### <mark style="color:yellow;">General Questions</mark>

<details>

<summary>Q: How do I open the skill tree menu?</summary>

A: You can open the menu in few ways:

* Using the command `/skill` (default)
* Pressing F7 (default keybind can be changed)
* Using the configured item if enabled
* Triggering the event `devhub_skillTree:client:openSkillTree`

</details>

<details>

<summary>Q: How do players earn skill points?</summary>

A: Players earn skill points in two ways:

1. Every time they level up (default 1 point per level)
2. Through admin commands or script exports

</details>

<details>

<summary>Q: How do players earn XP?</summary>

A: By default, XP is earned through:

* Running
* Swimming
* Melee combat
* Shooting
* Driving Additional XP sources can be added through exports.

</details>

<details>

<summary>Q: Can I reset my skills?</summary>

A: Yes, if enabled in the configuration. The reset can be:

* Free
* Cost money
* Require an item This is configurable in `Config.SkillReset`.

</details>

<details>

<summary>Q: In the standard version can I use skills from other DEVHUB scripts ?</summary>

A: YES , both version can do it.

</details>

<details>

<summary>Q: In the standard version can I add new skills via config without using generator ?</summary>

A: NO , the ability to add your own skill tree via config or generator is only available in the <mark style="color:green;">**exclusive version**</mark>.

</details>

<details>

<summary>Q: I want to upgrade the standard to exclusive version, can I get a discount?</summary>

A: SURE, Create a ticket on our discord and we will give you a discount code for the exclusive version.

</details>

### <mark style="color:yellow;">Technical Questions</mark>

<details>

<summary>Q: How do I add a new skill category?</summary>

A: New skill categories can only be added in the exclusive version:

1. Use the in-game generator
2. Create your skill layout
3. Copy the generated config
4. Add it to your config file

</details>

<details>

<summary>Q: Can I modify skill effects?</summary>

A: Yes, there are two ways:

1. Change the `effect` value in the skill configuration
2. Use the skill generator (exclusive version) to modify effects

</details>

<details>

<summary>Q: How do I integrate skills with other scripts?</summary>

A: You can use:

1. Event listeners (`devhub_skillTree:client:listener:skillUnlocked`)
2. Exports (`hasUnlockedSkill`, `getSkillEffect`)
3. Server-side exports for checking skill status

</details>

<details>

<summary>Q: What's the difference between standard and exclusive versions?</summary>

A: Exclusive version includes:

* Skill tree generator
* Ability to create custom skill categories
* Live preview of skill tree changes
* More customization options

</details>

<details>

<summary>Q: What does effect do?</summary>

A: The effect option allows developers to centralize skill effect values, simplifying long-term script maintenance. By defining effects in one place, you can adjust skill-related attributes more efficiently without needing to modify multiple scripts. However, it is not mandatory to use this feature; it is entirely up to your preference!

</details>

<details>

<summary>Q: Can I block certain skills path when I unlock other skills?</summary>

A: Yes, you can use our built-in skill tree generator in the exclusive version to manage skill path blocking:

1. Select the skill you want to set as a blocker
2. Click on "Block skills after unlock" option
3. Select which skills should be blocked after this skill is unlocked
4. The selected skills will become unavailable after the player unlocks the blocking skill

This feature is useful for creating mutually exclusive skill paths or preventing certain skill combinations.

</details>

<details>

<summary>Q: Can an item or other action be required for player to unlock skill ?</summary>

A: Yes, you can do it using Config.UnlockHandlerForSkills in configs/sh.main.lua\
More on that you can find [here](https://docs.devhub.gg/skill-tree-1/configuration#custom-skill-unlock-requirements)

You can also use the new Helpers (v3) to simplify item/job/money checks.

</details>

<details>

<summary>Q: What is skill degradation? (v3)</summary>

A: An optional per-skill mechanic — players must keep earning XP to maintain unlocked skills. If a player stops earning XP in a category, that category's skills slowly lose "stamina" each cycle (default 1 day). When stamina hits 0 the skill effect is disabled until the player earns enough XP to reactivate it.

Configurable via `Config.SkillDegradation` and per-skill `degradation = {...}` blocks. Disable entirely with `Config.SkillDegradation = nil`.

</details>

<details>

<summary>Q: What is the daily XP limit and how do I disable it? (v3)</summary>

A: `Config.DailyXpLimit` caps how much XP a player can earn per day in each category. By default `DefaultLimit = 0` which means **unlimited**, so the system is effectively off out of the box. To fully disable the system (skip even the reset timer), set `Config.DailyXpLimit = nil`.

</details>

<details>

<summary>Q: What is the premium currency? (v3)</summary>

A: A global secondary currency you can require (or accept) to unlock skills — useful for donor/VIP currencies. Configured via `Config.PremiumCurrency` with a server-side `handler` that checks and deducts the currency. Skills opt-in by specifying a `premiumCurrency` amount via the generator. See the Premium Currency section in the configuration.

</details>

<details>

<summary>Q: How do I switch themes? (v3)</summary>

A: Set `Config.Theme` in `configs/sh.main.lua` to one of `"legacy"`, `"modern"`, `"zombie"`, or `"fantasy"` and restart the resource.

</details>

<details>

<summary>Q: How do I open the admin panel? (v3)</summary>

A: Open the skill tree menu while logged in as an admin — the admin button appears in the top right. Permission is checked via `Core.IsPlayerAdmin(source)` from `devhub_lib`, so make sure your framework's admin detection is configured.

</details>

### <mark style="color:yellow;">Common Issues</mark>

<details>

<summary>Q: I upgraded from v2 and players only earn 5 XP per action — what happened?</summary>

A: The `Config.EarnXp` format changed in v3. The XP amount moved from a top-level `xp = N` field into the value of `addTo`. The old format still runs, but `addTo = {['personal'] = true}` is no longer a number, so it silently falls back to **5 XP**.

**Old (v2):**

```lua
['running'] = { xp = 10, timeout = 10000, addTo = { ['personal'] = true } }
```

**New (v3):**

```lua
['running'] = { timeout = 10000, addTo = { ['personal'] = 10 } }
```

</details>

<details>

<summary>Q: Database error after v3 upgrade — missing column 'data' or 'name'</summary>

A: v3 adds two new columns to the `dh_skilltree` table. Run the SQL migration:

```sql
ALTER TABLE `dh_skilltree`
ADD COLUMN IF NOT EXISTS `data` LONGTEXT DEFAULT NULL,
ADD COLUMN IF NOT EXISTS `name` VARCHAR(255) DEFAULT NULL;
```

</details>

<details>

<summary>Q: Skill category from external script like mining is not working?</summary>

A: Make sure to:

1. Ensure the external script AFTER skill tree in your server.cfg
2. If you've restarted skill tree, you must also restart the desired script so skills can be added again
3. This is because external skills are registered on script start

</details>

<details>

<summary>Q: Skills aren't saving after server restart</summary>

A: Check:

1. Database connection
2. SQL file installation
3. Resource load order (devhub\_lib must load first)

</details>

<details>

<summary>Q: When I join server my unlocked skills are not working, or skill tree is not opening?</summary>

A: Most likely you haven't configured devhub\_lib playerLoaded event. Make sure to properly configure your framework's playerLoaded event in devhub\_lib. This is required for the skill tree to initialize properly when a player joins the server.<br>

</details>

<details>

<summary>Q: How do I block certain skills based on jobs?</summary>

A: Edit CategoryVisibilityHandler in generator

</details>

### <mark style="color:yellow;">Performance Questions</mark>

<details>

<summary>Q: Will this impact server performance?</summary>

A: The impact is minimal because:

* Skills are loaded only when needed
* Effects are calculated client-side
* Database operations are optimized
* Events are throttled

</details>

<details>

<summary>Q: How many skills can I add?</summary>

In v1 and v2 each category supports up to 171 slots (9x19 grid)\
In v3 there are no limits

</details>

<details>

<summary>Q: Does it support ESX/QBCore/Custom?</summary>

A: Yes, through devhub\_lib which provides framework configuration. Configure your framework in devhub\_lib settings.

</details>

### <mark style="color:yellow;">Support</mark>

<details>

<summary>Q: I found a bug, what should I do?</summary>

A: Follow these steps:

1. Check the documentation
2. Update to latest version
3. Join Discord support
4. Create detailed bug report

</details>
