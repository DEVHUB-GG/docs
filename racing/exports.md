# 📚 Exports



```lua
local raceId = exports['devhub_racing']:createRace({
    mapId = 1,              -- required: map ID (number) or "random"
    startDelay = 300,       -- optional: seconds until race starts (default: 60)
    laps = 3,               -- optional: laps for loop maps (default: 3)
    collisions = true,      -- optional: player vehicle collisions (default: true)
    fpv = false,            -- optional: force first person view (default: false)
    entryFee = 500,         -- optional: entry fee (default: 0)
    vehicleClass = "ALL",   -- optional: "ALL", "S", "A", "B", "C", "D" (default: "ALL")
    winnerReward = 10000,   -- optional: money reward for winner (default: 0)
    official = true,        -- optional: MMR-ranked race (default: false)
    creatorIdentifier = "SYSTEM", -- optional: creator identifier (default: "SYSTEM")
})

-- @param data (table) - Race configuration:
--   data.mapId          (number|string) [REQUIRED] - The map ID to use, or "random" for a random verified map
--   data.startDelay     (number)        [OPTIONAL] - Seconds until race starts (default: 60)
--   data.laps           (number)        [OPTIONAL] - Number of laps for loop maps (default: 3)
--   data.collisions     (boolean)       [OPTIONAL] - Enable player vehicle collisions (default: true)
--   data.fpv            (boolean)       [OPTIONAL] - Force first person view (default: false)
--   data.entryFee       (number)        [OPTIONAL] - Entry fee amount (default: 0)
--   data.vehicleClass   (string)        [OPTIONAL] - Vehicle class restriction: "ALL", "S", "A", "B", "C", "D" (default: "ALL")
--   data.winnerReward   (number)        [OPTIONAL] - Money reward for winner (default: 0)
--   data.official       (boolean)       [OPTIONAL] - Whether the race is official / MMR-ranked (default: false)
--   data.creatorIdentifier (string)     [OPTIONAL] - Identifier of the creator (default: "SYSTEM")

-- @return raceId (number|false) - The created race ID, or false on failure
```
