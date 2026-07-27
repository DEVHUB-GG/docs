---
description: The things people ask about most, and the fix for each.
---

# 🩺 Troubleshooting

***

## <mark style="color:yellow;">**Setup**</mark>

<details>

<summary><code>/admindevhub</code> does nothing</summary>

You do not have the ACE permission. The creator is gated by `command`, the same permission every DevHub script uses.

```javascript
add_ace group.admin command allow
add_principal identifier.license:YOUR_LICENSE group.admin
```

The server also rejects every creator action from a player without it, so forcing the interface open gets you nowhere.

</details>

<details>

<summary>The machines are invisible, or they show up as a red question mark</summary>

`devhub_gym2_assets` and `devhub_gym2_assets_anim` are not started. Both are required and both have to be in your `server.cfg`:

```javascript
ensure devhub_gym2_assets
ensure devhub_gym2_assets_anim
ensure devhub_gym2
```

</details>

<details>

<summary>Nothing works at all after I renamed the resource</summary>

Rename it back to `devhub_gym2`. The resource name is baked into the escrow, the exports and the NUI callbacks.

</details>

<details>

<summary>Do I have to import the SQL file?</summary>

No. The script creates its tables and inserts the defaults on first start. Importing `sql/install.sql` by hand does exactly the same thing and is safe to run twice, so use it if you prefer to see the schema first.

</details>

***

## <mark style="color:yellow;">**Building a gym**</mark>

<details>

<summary>I placed equipment but other players cannot see it</summary>

Press **Save** in the Zone Editor. Placements are stored to the database immediately, so you never lose one, but the zone is only pushed out to everyone else when you save.

</details>

<details>

<summary>My zone settings keep reverting</summary>

You are leaving the editor without saving. Only confirmed placements survive on their own. The zone name, memberships, the blip, removed props, interaction points and ped-clear areas all wait for **Save**.

</details>

<details>

<summary>The machine has no target option on it</summary>

Two usual causes:

* The equipment has no exercise ticked. Open the pen editor on the row and check **Exercises on this prop**. With none ticked the prop is decoration on purpose.
* The zone requires a membership and you do not have one. A member-only gym turns non-members away at the machine.

</details>

<details>

<summary>I cannot find the Boss Menu feature on interaction points</summary>

It only appears when the zone is assigned to a business. Assign it first from the business admin panel, then come back.

</details>

<details>

<summary>NPCs still walk through my gym</summary>

Check three things on the **Other** tab: ped clearing is switched on, at least one area exists, and the area's radius actually covers the room. Use the eye button to draw the area in the world and press `TAB` to look at it. Then save.

</details>

<details>

<summary>The map blip is not there</summary>

Switch the blip on, set its coordinates (the **My Position** button is the easy way), then press **Save**. A blip with no coordinates is not drawn.

</details>

***

## <mark style="color:yellow;">**Tuning and configuration**</mark>

<details>

<summary>I changed something in Gym Config and nothing happened</summary>

That page does not live-reload. Restart the resource. The interface says so above the tabs.

The tutorial toggle is the one exception and applies immediately.

</details>

<details>

<summary>I added a supplement and it does nothing</summary>

Two steps are needed, and skipping either one breaks it:

1. The item must exist in your inventory, with an image. See the items step in [installation.md](installation.md "mention").
2. The item must be listed on the **Supplement Items** page, and the resource restarted afterwards.

</details>

<details>

<summary>The interface shows raw text like <code>notify_exercise_finished</code></summary>

That key is missing or misspelled in `configs/translation.lua`. A key the script cannot find is printed as itself, which is the point: it is obviously wrong rather than quietly blank.

</details>

<details>

<summary>My translation shows garbled characters</summary>

Save `translation.lua` as **UTF-8**.

</details>

<details>

<summary>Skill tree perks are not doing anything</summary>

* `Shared.DevhubSkillTreeEnabled` has to be `true` in `configs/shared.lua`.
* `devhub_skillTree` has to be started **before** `devhub_gym2` in your `server.cfg`.

</details>

***

## <mark style="color:yellow;">**Gameplay questions**</mark>

<details>

<summary>Players say the minigames are too hard</summary>

Three separate dials, in the order you should try them:

1. Lower the difficulty range on the machine's minigame panel. Setting both sliders to 1 or 2 makes every rep gentle.
2. Assign fewer minigames, or none at all. With none, reps simply tick by.
3. Drop the **Global Multiplier** on the Fatigue section of Gym Config so sets last longer.

Test them yourself first on the **Minigames & Presets** page.

</details>

<details>

<summary>A player is permanently "too tired"</summary>

Fatigue drains per muscle, per minute. Raise **Default Recovery/min** on the Fatigue section, or the per-muscle **Recovery/min** on the Body Parts tab, and restart the resource. **Max Threshold** decides at what percentage a muscle counts as exhausted.

</details>

<details>

<summary>Which muscle blocks an exercise?</summary>

The first body part with a fatigue weight above 0 in that exercise's **Fatigue distribution**. That is the muscle named in the "too tired" message. Reorder the weights on the Exercises tab of Gym Config if you want a different one to gate it.

</details>

<details>

<summary>A member cannot claim the daily reward</summary>

* The reward has to be configured and switched on for that gym.
* The player needs an active membership.
* It can be claimed once every 24 hours, and the interface shows the time left.
* If the gym belongs to a business, the reward item must be in stock in that business's warehouse. Every claim takes one off the shelf.

</details>

<details>

<summary>Nobody can spot my players</summary>

Spotting only exists when the zone belongs to a business, and only employees of **that** business can do it. The player also has to be mid-set at the time.

</details>
