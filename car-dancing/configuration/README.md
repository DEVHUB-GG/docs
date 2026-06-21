# 🛠️ Configuration

## <mark style="color:yellow;">**server.lua**</mark>

```lua
-- Config.OpenMenuOption can be either "command" or "item".
-- If set to "item", the menu will open upon using an item.
-- If set to "command", the menu will open through a command.
Config.OpenMenuOption = "command" -- Example assignment, change as needed.

-- Config.MenuCommand is the command used to open the menu.
-- This is only used if Config.OpenMenuOption is set to "command".
Config.MenuCommand = "battle" -- Example assignment, change as needed.

-- Config.MenuItem is the item used to open the menu.
-- This is only used if Config.OpenMenuOption is set to "item".
Config.MenuItem = "battle_creator" -- Example assignment, change as needed.
```

***

## <mark style="color:yellow;">**shared.lua**</mark>

```lua
-- Difficulty options: easy, medium, hard
Shared.Sounds = {
    { name = "Spanish song", time = "1:25", url = "spanish.ogg", difficulty = "hard" },
    { name = "Folk song", time = "0:25", url = "pietruszka.ogg", difficulty = "easy" },
}

Shared.Lang = {
    ['VolumeChange'] = "Page up/down : Volume %{percent}%",
    ['Points'] = "POINTS ",
    ['carDancing'] = "Car dancing",
    ['Refresh'] = "Refresh",
    ['SelectBet'] = "Select bet",
    ['MusicNotSelected'] = "Music not selected",
    ['Settings'] = "Settings",
    ['YourTeam'] = "Your team",
    ['PlayersNearby'] = "Players nearby",
    ['PlayersNotFound'] = "No players found",
    ['Abort'] = "Abort",
    ['Start'] = "Start",
    ['Invitation'] = "You are invited to car dance battle",
    ['Leader'] = "Leader",
    ['SongName'] = "Song name",
    ['Difficulty'] = "Difficulty",
    ['Bet'] = "Bet",
    ['Reject'] = "Reject",
    ['Accept'] = "Accept",
    ['StreakMultiplierActivator'] = "Streak multiplier activator",
    ['StreakMultiplier'] = "Streak multiplier",
    ['MissNegativePoints'] = "Miss negative points",
    ['RandomizedButtons'] = "Randomized buttons",
    ['MirrorImage'] = "Mirror image",
    ['Bombs'] = "Bombs",
    ['On'] = "On",
    ['Off'] = "Off",
    ['EscClose'] = "ESC to close",
    ['MusicPlayed'] = "Music Played: ",
    ['MaxStreak'] = "Max streak",
    ['Score'] = "Score",
    ['Name'] = "Name",
    ['Place'] = "Place",
    ['Looser'] = "LOOSER",
    ['Winner'] = "WINNER",
    ['LobbyWaiting'] = "Waiting for game to start<br><kbd>H</kbd> to leave lobby",
    ["CantInviteMorePlayers"] = "You can't invite more players!",
    ["PlayerInvited"] = "Player invited to game!",
    ["CantSetNegativeBet"] = "You can't set negative bet!",
    ["InviteExpired"] = "Invite expired!",
    ["GameStarted"] = "Game already started!",
    ["DontEnoughMoney"] = "You don't have enough money!",
    ['Nah'] = "Nah", 
    ['CouldBeBetter'] =  "It could be better", 
    ['NotBad'] = "Not bad", 
    ['KeepGoing'] = "Keep going", 
    ['YouGotThis'] = "You got this", 
    ['Good'] = "Good", 
    ['Nice'] = "Nice", 
    ['Great'] = "Great", 
    ['Awesome'] = "Awesome", 
    ['VeryGood'] = "Very good", 
    ['Super'] = "Super", 
    ['Amazing'] = "Amazing", 
    ['Perfect'] = "Perfect", 
    ['Incredible'] = "Incredible", 
    ['Unbelievable'] = "Unbelievable", 
    ['Godlike'] = "Godlike", 
    ['Bomb'] = "Bomb", 
    ['Miss'] = "Miss", 
}
```

* **Shared.Sounds:** List of music tracks available for car dancing.
  * **name:** Display name of the song.
  * **time:** Duration of the song.
  * **url:** Filename of the song in the `dh_cardance/html/song` folder.
  * **difficulty:** Difficulty level of the song. Options: `"easy"`, `"medium"`, `"hard"`.

### How to generate beat ?

{% content-ref url="beat-generating.md" %}
[beat-generating.md](beat-generating.md)
{% endcontent-ref %}
