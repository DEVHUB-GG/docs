---
description: Documentation for sh.skillTree.lua Configuration
---

# sh.skillTree.lua

Connects Petty Crimes to [devhub\_skillTree](https://store.devhub.gg/product/6440792). The script runs perfectly fine without it — leave the master switch off if you do not own the skill tree.

## <mark style="color:yellow;">**Skill Tree Enabled**</mark>

```lua
Config.SkillTreeEnabled = false -- master switch; the script runs fine without the tree
```

* **Description**: Turns the whole skill-tree integration on. When `false`, no category is registered, no XP is handed out and no perk affects a crime.

***

## <mark style="color:yellow;">**Skills Category**</mark>

```lua
Config.SkillsCategory = {
    skill = 'pettyCrimes',
    title = 'Petty Crimes',
}
```

* **skill**: The category key registered in the skill tree. Keep it unique across your server.
* **title**: The category name players see in the skill tree UI.

***

## <mark style="color:yellow;">**Skill Tree XP**</mark>

```lua
Config.SkillTreeXp = {
    default = 10,
    perActivity = {
        postalBoxTheft    = 8,
        parkingMeterTheft = 8,
        phoneBoothTheft   = 8,
        carRadioTheft     = 12,
        npcPickpocket     = 10,
        npcMugging        = 15,
        porchTheft        = 12,
        smashAndGrab      = 15,
    },
}
```

* **Description**: XP handed to the skill tree after each committed crime. Skill points come from levelling up in the tree itself.
* **default**: Used for any activity not listed in `perActivity`.
* **perActivity**: XP per crime, keyed by the activity name. Raise the number to make that crime level the player faster.

***

## <mark style="color:yellow;">**Skill Caps**</mark>

```lua
Config.SkillCaps = {
    speed        = 60, -- max % the action time can be shortened by
    autocomplete = 50, -- max % chance the minigame is auto-solved
    lowprofile   = 75, -- max % the police-alert chance is reduced by
}
```

* **Description**: Hard ceilings on each perk's summed effect (in %), so a fully-levelled player can never reach an absurd value — for example a zero-length progress bar or a crime that can never be reported.
* **Example**: With `speed = 60`, a player who unlocked every speed perk still needs 40% of the original action time.

***

## <mark style="color:yellow;">**Skill Perks**</mark>

```lua
Config.SkillPerks = {
    speed = {
        title = 'Quick Hands',
        desc  = 'You work <strong>%s%% faster</strong> — the progress bar and the setup animation both take less time.',
        icon  = 'fa-solid fa-gauge-high',
        img   = 'https://upload.devhub.gg/dh_upload/pettyCrimes_speed.gif',
        tiers = { { effect = 8, points = 1 }, { effect = 12, points = 2 } },
    },
    autocomplete = { ... },
    lowprofile   = { ... },
}
```

The three perks every crime gets. Each crime receives a bundle of two of each perk (tier I + tier II) and the two `tiers` effects are **additive** — with the values above, both speed tiers unlocked means 8 + 12 = 20% faster, capped by `Config.SkillCaps`.

* **title**: Perk name shown in the tree.
* **desc**: Perk description. `%s` is replaced with that tier's `effect` number, and HTML is allowed.
* **icon**: FontAwesome class for the perk node.
* **img**: Image/GIF shown on the perk tooltip.
* **tiers**: The two tiers of the perk.
  * **effect**: The perk's value in %, meaning depends on the perk (`speed` = shorter action time, `autocomplete` = chance the minigame is solved for the player and skipped, `lowprofile` = reduction of the police-alert chance).
  * **points**: Skill points needed to unlock that tier.

***

## <mark style="color:yellow;">**Skill Crimes**</mark>

```lua
Config.SkillCrimes = {
    { activity = 'postalBoxTheft',    label = 'Postal Box Theft' },
    { activity = 'parkingMeterTheft', label = 'Parking Meter Theft' },
    { activity = 'phoneBoothTheft',   label = 'Phone Booth Theft' },
    { activity = 'carRadioTheft',     label = 'Car Radio Theft' },
    { activity = 'npcPickpocket',     label = 'Pickpocketing' },
    { activity = 'npcMugging',        label = 'Mugging' },
    { activity = 'porchTheft',        label = 'Porch Theft' },
    { activity = 'smashAndGrab',      label = 'Smash & Grab' },
}
```

* **Description**: The crimes that get a skill bundle, in the order they appear in the tree.
* **activity**: The activity key the bundle belongs to.
* **label**: The caption shown above that crime's bundle.
* **Example**: Remove a line to drop that crime's bundle from the tree. A crime you disabled in the in-game editor is dropped automatically, whether it is listed here or not.

{% hint style="warning" %}
The best skill tree on FiveM, get it [here](https://store.devhub.gg/product/6440792)
{% endhint %}
