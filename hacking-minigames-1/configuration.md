# 🛠️ Configuration

## <mark style="color:yellow;">**Available Minigames**</mark>

### Circuit Minigame

**Game ID**: `circuit`

A puzzle-based minigame where players must complete electrical circuits by connecting nodes and paths.

<figure><img src="https://upload.devhub.gg/dh_upload/docs/hacking-minigames-1-1.webp" alt=""><figcaption></figcaption></figure>

***

### Enigma Minigame

**Game ID**: `enigma`

A sequence-based decryption challenge where players must decode encrypted patterns within a time limit.

<figure><img src="https://upload.devhub.gg/dh_upload/docs/hacking-minigames-1-2.webp" alt=""><figcaption></figcaption></figure>

***

### Morse Code Minigame

**Game ID**: `morse`

A communication-based challenge where players must decode morse code messages using dots and dashes.

<figure><img src="https://upload.devhub.gg/dh_upload/docs/hacking-minigames-1-3.webp" alt=""><figcaption></figcaption></figure>

***

### Bruteforce Minigame

**Game ID**: `bruteforce`

A password cracking simulation where players must break through security systems using various techniques.

<figure><img src="https://upload.devhub.gg/dh_upload/docs/hacking-minigames-1-4.webp" alt=""><figcaption></figcaption></figure>

***

### Tuning Minigame

**Game ID**: `tuning`

A frequency-based minigame where players must tune into the correct frequency or signal.

<figure><img src="https://upload.devhub.gg/dh_upload/docs/hacking-minigames-1-5.webp" alt=""><figcaption></figcaption></figure>

***

## <mark style="color:yellow;">**Command Configuration**</mark>

```lua
Config.CommandEnabled = true
```

* **Description**: Controls whether the test command for minigames is enabled or disabled.
* **Default**: `true`
* **Usage**: When enabled, allows the use of `/startMinigame` command for testing purposes.

***

## <mark style="color:yellow;">**Command Usage**</mark>

When `Config.CommandEnabled` is set to `true`, you can use the following command:

```
/startMinigame [minigame] [time]
```

**Parameters:**

* **minigame**: The type of minigame to start
  * Available options: `circuit`, `enigma`, `morse`, `bruteforce`, `tuning`
* **time**: Time limit in seconds (default: 60)

**Examples:**

```
/startMinigame circuit 60
/startMinigame enigma 45
/startMinigame morse 30
```

***

## <mark style="color:yellow;">**Export Function**</mark>

```lua
local result = exports['devhub_hackingMinigames']:startMinigame(minigame, time)
```

* **Description**: Programmatically start a minigame from another resource.
* **Parameters**:
  * `minigame` (string): The minigame type to start
  * `time` (number): Time limit in seconds
* **Returns**: Boolean indicating success or failure of the minigame
* **Example Usage**:

```lua
local success = exports['devhub_hackingMinigames']:startMinigame("circuit", 60)
if success then
    print("Player successfully completed the minigame!")
else
    print("Player failed the minigame.")
end
```

***

## <mark style="color:yellow;">**Integration**</mark>

To integrate these minigames into your scripts:

1. **Basic Integration**:

```lua
local result = exports['devhub_hackingMinigames']:startMinigame("circuit", 45)
if result then
    -- Player succeeded
    -- Grant rewards, continue mission, etc.
else
    -- Player failed
    -- Handle failure, retry option, etc.
end
```

2. **Advanced Integration EXAMPLE**:

```lua
-- Before starting a heist hacking sequence
if PlayerHasHackingDevice() then
    local minigameType = GetRandomMinigame() -- Your custom function
    local timeLimit = CalculateTimeLimit() -- Based on player skill, etc.
    
    local success = exports['devhub_hackingMinigames']:startMinigame(minigameType, timeLimit)
    
    if success then
        TriggerEvent('heist:hackingSuccess')
    else
        TriggerEvent('heist:hackingFailed')
    end
end
```

***

## <mark style="color:yellow;">**Sound Configuration**</mark>

<mark style="color:$primary;">**Path: html/config.js**</mark>

```lua
window.config = {
    soundVolume: 0.5,
};
```

* **Description**: Controls the volume level for minigame sound effects.
* **Range**: 0.0 to 1.0 (0% to 100%)
* **Default**: 0.5 (50%)

***
