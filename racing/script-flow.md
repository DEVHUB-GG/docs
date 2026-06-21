---
description: This guide explains how the racing system works from start to finish.
---

# 🌊 Script Flow

## <mark style="color:yellow;">Map Creation Phase</mark>

### Player initiates map creator

1. Player opens the racing app in their laptop.
2. Navigates to "MAP CREATOR" section.
3. Fills in map configuration (name, race type: Sprint/Loop).
4. Opens the map editor interface.

### Map building process

1. Player places **start line** using available props (flags, gantries).
2. Player places **checkpoints** along the desired race route (minimum 1 required).
3. Player places **finish line** (can be same location as start for loop races).
4. Optionally places **decoration props** (barriers, lights, hay bales) for track aesthetics.
5. Props can be rotated with `R` key and distance adjusted with mouse scroll.
6. Player saves the map when complete.

### Map validation

1. System checks that start, finish, and at least one checkpoint are configured.
2. Map is saved to database with creator information.
3. If admin permissions are required, only admins can create maps.
4. Maps can be verified by admins to enter official race pool.

***

## <mark style="color:yellow;">Race Creation Phase</mark>

### Player creates a race

1. Player navigates to "MAPS" section in racing app.
2. Selects a map from created maps.
3. Clicks "Create Race" button.
4. Configures race parameters:
   * **Entry Fee**: Amount players must pay to join (cash/bank configurable).
   * **Start Delay**: Time until race starts (1min, 5min, 30min, 1h, 5h, 24h).
   * **Vehicle Class**: ALL, S, A, B, C, or D (restricts allowed vehicles).
   * **Collisions**: Enable/disable player vehicle collisions.
   * **First Person View**: Force FPV during race.
   * **Laps**: Number of laps (for loop-type maps only).
5. Race is created and becomes visible in "RACES" list and in game if it is the closest to start.

### Official races (automated)

1. System automatically generates official races based on `Config.OfficialRaceGenerator`.
2. Official races are scheduled daily at configured times.
3. Can use specific maps or randomly select from verified maps pool.
4. Official races award MMR points and optional winner rewards.
5. Discord webhook announces new official races (if configured).

***

## <mark style="color:yellow;">Race Joining Phase</mark>

### Player joins a race

1. Player must be in a vehicle to join.
2. System validates:
   * Player has sufficient funds for entry fee (if required).
   * Player's vehicle class matches race requirements.
   * Player has unlocked required vehicle class skill (if skill tree enabled).
   * Player is not already in another race.
3. Entry fee is deducted (cash or bank based on config).
4. Player enters waiting room state.

### Joining restrictions

* Official races: Joining restricted to 30+ minutes before start time.
* Players can press `E` to leave waiting room before race starts.
* Players who leave their vehicle are automatically removed from race.
* Vehicle class validation occurs at join time and race start time.

***

## <mark style="color:yellow;">Pre-Race Phase</mark>

### Waiting period

1. Players in waiting room see countdown timer to race start.
2. Players are teleported to assigned starting lanes.
3. System checks vehicle class compatibility (removes invalid players).
4. Race music begins playing (if enabled and not muted by player).
5. Routing bucket is applied (isolates race instance if enabled).
6. Local traffic is disabled (if configured).

### Countdown sequence

1. "Be ready!" notification displayed.
2. Visual countdown on screen: 3... 2... 1... GO!
3. Countdown sound effects play.
4. Race officially begins.

***

## <mark style="color:yellow;">Active Race Phase</mark>

### Racing mechanics

1. Checkpoint blips appear on map for navigation.
2. 3D checkpoint markers visible in-game (if enabled).
3. Player position syncs with other racers every 1 second (configurable).
4. UI displays:
   * Current checkpoint / lap information.
   * Race progress (checkpoint X of Y).
   * Current position in race.
   * Distance to next checkpoint.
   * Player rankings (sidebar).

### Checkpoint progression

1. Player drives through checkpoint trigger zones.
2. Next checkpoint blip updates.
3. Lap counter increments (loop races only).
4. Client function `CheckpointPassed` is triggered for custom logic.

### Race validation

* Players must pass through checkpoints in order.
* Shortcuts detected by checkpoint sequencing.
* Missing checkpoints invalidate race completion.

***

## <mark style="color:yellow;">Race Finish Phase</mark>

### Individual player finish

1. Player crosses finish line after completing all checkpoints/laps.
2. Finish time is recorded.
3. Position is determined (1st, 2nd, 3rd, etc.).
4. Fireworks sound effect plays (for podium finishes).
5. "RACE FINISHED" screen displays with results.

### Reward calculation

1. **Entry fee pool**: Distributed based on position (multiplied by config setting).
2. **Winner reward**: Only for official races (if configured).
3. **MMR points**: Awarded for official races or if `AllowMrrOnAllRaces` is enabled.
   * 1st place: +50 MMR (base)
   * 2nd place: +25 MMR (base)
   * 3rd place: +10 MMR (base)
   * Below 3rd: -25 MMR × (position - 3)
4. **XP points**: Skill tree XP awarded (if skill tree enabled).
   * 1st: 100 XP
   * 2nd: 50 XP
   * 3rd: 25 XP
   * 4th: 10 XP
   * 5th: 5 XP
5. **Achievement progress**: Tracked for milestone achievements.

### Post-race cleanup

1. Players teleported back to start position (if configured).
2. Routing bucket removed (players return to normal dimension).
3. Local traffic re-enabled.
4. Race music fades out.
5. Race data saved to database (history, statistics).

***

## <mark style="color:yellow;">Leaderboard and Statistics Phase</mark>

### Data tracking

1. Player profile updated with race results.
2. MMR history recorded (last 7 entries by default).
3. Race history recorded (last 5 entries by default).
4. Map statistics updated (play count, ratings).
5. Global leaderboard refreshed (cached for 15 minutes).

### Profile display

* Total races completed
* Total wins
* Podium finishes
* Average position
* Average race time
* MMR score and history graph
* Medals/achievements earned
