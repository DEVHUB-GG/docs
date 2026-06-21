# 🩸 Degradation

### <mark style="color:yellow;">Overview</mark>

Skill degradation is an **optional** mechanic that gives unlocked skills a "stamina" pool which decays over time. If a player stops earning XP in a category, their unlocked skills slowly lose degradation XP each cycle. When the pool hits 0, the skill effect is **disabled** until the player earns enough XP back to reactivate it.

It's designed for servers that want skills to feel like a maintenance investment rather than a one-time unlock.

{% hint style="info" %}
Set `Config.SkillDegradation = nil` to disable the system entirely. Skills without a `degradation` block are also exempt — opt-in per skill.
{% endhint %}

***

### <mark style="color:yellow;">How it works</mark>

1. When a player unlocks a skill with a `degradation` block, the skill's `degradationXp` is set to `maxDegradationXp`.
2. Every `CycleInterval` days, `removePerCycle` XP is subtracted from `degradationXp` for each unlocked skill in that category.
3. When `degradationXp` hits 0:
   * The skill effect is **disabled** (treated as not-unlocked for effect calculations)
   * `devhub_skillTree:client:listener:skillDegraded` fires
4. Any XP the player earns in the **same category** also restores degradation XP by the same amount.
5. When degradation XP reaches `reactivationThreshold`, the skill effect comes back online:
   * `devhub_skillTree:client:listener:skillDegradedRecovered` fires

Cycles are applied:

* **On player load** — based on the stored `lastDegradationCycle` timestamp
* **Every hour** for online players whose cycle interval has elapsed

The `MaxMissedCycles` setting caps how many cycles can be applied at once after a long offline period — so players who take a break don't lose all their degradation XP at once.

***

### <mark style="color:yellow;">Global configuration (sh.main.lua)</mark>

```lua
Config.SkillDegradation = {
    CycleInterval   = 1,   -- float | Default degradation cycle interval in days (1 = every 24h)
    MaxMissedCycles = 30,  -- int   | Max cycles applied at once when a player reconnects
    Categories      = {},  -- per-category overrides (see below)
}
```

#### Per-category overrides

Override the global defaults for specific categories. Any category not listed here uses the defaults above.

```lua
Config.SkillDegradation = {
    CycleInterval = 1,
    MaxMissedCycles = 30,
    Categories = {
        ['police'] = {
            CycleInterval = 7,     -- police skills decay weekly instead of daily
            MaxMissedCycles = 4,
        },
        ['personal'] = {
            CycleInterval = 0.5,   -- personal skills decay every 12h
        },
    },
}
```

***

### <mark style="color:yellow;">Per-skill configuration (sh.skills.lua)</mark>

Add a `degradation` block to any skill that should be subject to degradation:

```lua
{
    title = "Speed Boost I",
    uid = "runFaster_1",
    -- ... other fields
    degradation = {
        maxDegradationXp = 1000,      -- Maximum degradation XP (starts full on unlock)
        removePerCycle = 100,         -- XP removed each cycle
        reactivationThreshold = 500,  -- XP needed to reactivate after depletion (0 = reactivates immediately)
    },
},
```

In the exclusive version, this block can also be edited per-skill from the generator UI.

{% hint style="info" %}
**`reactivationThreshold = 0`** means the skill reactivates the moment any XP is earned in the category. Use a higher value if you want the player to "earn back" the skill.
{% endhint %}

***

### <mark style="color:yellow;">Events</mark>

Both client and server versions are fired (each can be disabled via `Config.DisabledListeners`).

#### Client

```lua
RegisterNetEvent('devhub_skillTree:client:listener:skillDegraded', function(categoryUid, skillUid)
    -- Skill effect just got disabled
end)

RegisterNetEvent('devhub_skillTree:client:listener:skillDegradedRecovered', function(categoryUid, skillUid)
    -- Skill effect is back online
end)
```

#### Server

```lua
RegisterNetEvent('devhub_skillTree:server:listener:skillDegraded', function(source, categoryUid, skillUid)
    -- Skill effect just got disabled for `source`
end)

RegisterNetEvent('devhub_skillTree:server:listener:skillDegradedRecovered', function(source, categoryUid, skillUid)
    -- Skill effect is back online for `source`
end)
```

***

### <mark style="color:yellow;">Disabling the system</mark>

* **Disable globally** — set `Config.SkillDegradation = nil` (or remove / comment out the block)
* **Disable for a specific skill** — remove its `degradation = {...}` block
* **Disable for a specific category** — make sure no skill in that category has a `degradation` block
