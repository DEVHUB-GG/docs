---
description: >-
  The skill generator and adding your own skills is only available in the
  exclusive version
---

# ⚙️ Skill Generator

{% hint style="danger" %}
Make sure `devhub_skillTree_generator` is started and `Config.EnableGenerator` is set to <mark style="color:green;">**true**</mark>
{% endhint %}

{% stepper %}
{% step %}
#### Open Admin Panel
{% endstep %}

{% step %}
#### A new button will appear in the right top corner
{% endstep %}

{% step %}
#### Configure category settings
{% endstep %}

{% step %}
#### Create new skills by clicking at any box on the grid
{% endstep %}

{% step %}
#### Make sure that not errors are found
{% endstep %}

{% step %}
#### Confirm generation of file

Changes will be <mark style="color:$success;">automatically</mark> applied
{% endstep %}

{% step %}
#### Create your skill functionality using our export and listeners
{% endstep %}

{% step %}
#### Restart script


{% endstep %}
{% endstepper %}

***

### <mark style="color:yellow;">New in v3</mark>

The generator UI has been expanded with new editors for everything you previously had to hand-edit in config files:

* **Visibility handlers** — edit per-category visibility filters that get saved to `Config.CategoryVisibilityHandler`
* **Unlock handlers** — per-skill unlock conditions saved to `Config.UnlockHandlerForSkills`, with support for the new Helpers
* **Trigger on unlock** — auto-fire your own events when a skill is unlocked, saved to `Config.TriggerOnSkillUnlock`
* **Premium currency** — configure the global premium currency directly from the UI
* **Daily XP limits** — set per-category caps that write to `Config.DailyXpLimit.Limits`
* **Degradation overrides** — per-category overrides written to `Config.SkillDegradation.Categories`
* **Category groups & icons** — assign `group` and `icon` to organize categories in the menubar

***

### <mark style="color:yellow;">Automatic Config Backups</mark>

Every time you save from the generator, the affected config files (`sh.main.lua`, `sh.skills.lua`, `s.main.lua`) are backed up first. You can list, restore, or delete backups from the admin panel.

```lua
Config.MaxBackups = 5 -- int | Maximum backups to keep (0 = unlimited)
```

{% hint style="info" %}
Backups are stored on disk under the resource's `backups/` folder with a JSON index. Restoring a backup also creates a safety backup of the current state first.
{% endhint %}

***
