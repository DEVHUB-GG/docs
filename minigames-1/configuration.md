# 🛠️ Configuration

## <mark style="color:yellow;">**Available Minigame**</mark>

**Game ID**: minigame\_1

```lua
        settings = {
            ['easy'] = {
                boxes = 3,
                moveSpeed = 2,
            },
            ['medium'] = {
                boxes = 5,
                moveSpeed = 3,
            },
            ['hard'] = {
                boxes = 8,
                moveSpeed = 5,
            },
 }
```

<figure><img src="https://upload.devhub.gg/dh_upload/docs/minigames-1-1.webp" alt=""><figcaption></figcaption></figure>

***



**Game ID**: minigame\_2

```lua
        settings = {
            ['easy'] = {
                boxes = 3,
                moveSpeed = 2,
            },
            ['medium'] = {
                boxes = 5,
                moveSpeed = 3,
            },
            ['hard'] = {
                boxes = 8,
                moveSpeed = 5,
            },
}
```

<figure><img src="https://upload.devhub.gg/dh_upload/docs/minigames-1-2.webp" alt=""><figcaption></figcaption></figure>

***

**Game ID**: minigame\_3

```lua
settings = {
            ['easy'] = {
                time = 5,
                keysToPress = 2,
                leftHandKeys = {"Q", "W", "E", "R", "T", "A", "S", "D", "F", "G", "Z", "X", "C", "V", "B"},
                rightHandKeys = {"Y", "U", "I", "O", "P", "H", "J", "K", "L", "N", "M"},
            },
            ['medium'] = {
                time = 10,
                keysToPress = 4,
                leftHandKeys = {"Q", "W", "E", "R", "T", "A", "S", "D", "F", "G", "Z", "X", "C", "V", "B"},
                rightHandKeys = {"Y", "U", "I", "O", "P", "H", "J", "K", "L", "N", "M"},
            },
            ['hard'] = {
                time = 15,
                keysToPress = 6,
                leftHandKeys = {"Q", "W", "E", "R", "T", "A", "S", "D", "F", "G", "Z", "X", "C", "V", "B"},
                rightHandKeys = {"Y", "U", "I", "O", "P", "H", "J", "K", "L", "N", "M"},
            },
  }
```

<figure><img src="https://upload.devhub.gg/dh_upload/docs/minigames-1-3.webp" alt=""><figcaption></figcaption></figure>

***

**Game ID**: minigame\_4

```lua
settings = {
            ['easy'] = {
                zoneWidth = 20,
                hitSpeed = 0.5,
                reduceOnHit = 10,
                regainSpeed = 2,
                keySequence = {"A", "D"},
            },
            ['medium'] = {
                zoneWidth = 20,
                hitSpeed = 0.75,
                reduceOnHit = 10,
                regainSpeed = 3,
                keySequence = {"A", "D"},
            },
            ['hard'] = {
                zoneWidth = 15,
                hitSpeed = 1,
                reduceOnHit = 10,
                regainSpeed = 4,
                keySequence = {"A", "D"},
            },
}
```

<figure><img src="https://upload.devhub.gg/dh_upload/docs/minigames-1-4.webp" alt=""><figcaption></figcaption></figure>

***

**Game ID**: minigame\_5

```lua
settings = {
            ['easy'] = {
                rounds = 3,
                sequenceStartLength = 2,
                showSpeed = 1000,
            },
            ['medium'] = {
                rounds = 5,
                sequenceStartLength = 3,
                showSpeed = 800,
            },
            ['hard'] = {
                rounds = 7,
                sequenceStartLength = 4,
                showSpeed = 600,
            },
 }
```

<figure><img src="https://upload.devhub.gg/dh_upload/docs/minigames-1-5.webp" alt=""><figcaption></figcaption></figure>

***

**Game ID**: minigame\_6

```lua
settings = {
            ['easy'] = {
                targetScore = 10,
                gameTime = 25000,
                targetLifetime = 2500,
                spawnDelay = 1000,
            },
            ['medium'] = {
                targetScore = 15,
                gameTime = 25000,
                targetLifetime = 2000,
                spawnDelay = 800,
            },
            ['hard'] = {
                targetScore = 20,
                gameTime = 25000,
                targetLifetime = 1500,
                spawnDelay = 600,
            },
}
```

<figure><img src="https://upload.devhub.gg/dh_upload/docs/minigames-1-6.webp" alt=""><figcaption></figcaption></figure>

***

**Game ID**: minigame\_7

```lua
settings = {
            ['easy'] = {
                gameTime = 60000,
                numberOfWires = 3,
                difficulty = 'easy',
            },
            ['medium'] = {
                gameTime = 45000,
                numberOfWires = 4,
                difficulty = 'medium',
            },
            ['hard'] = {
                gameTime = 30000,
                numberOfWires = 5,
                difficulty = 'hard',
            },
 }
```

<figure><img src="https://upload.devhub.gg/dh_upload/docs/minigames-1-7.webp" alt=""><figcaption></figcaption></figure>

***

**Game ID**: minigame\_8

```lua
settings = {
            ['easy'] = {
                gameTime = 90000,
                numberOfPins = 3,
                difficulty = 'easy',
                sweetSpotRange = 35,
                sweetSpotSpeed = 0.4,
                pressureSpeed = 2,
            },
            ['medium'] = {
                gameTime = 60000,
                numberOfPins = 4,
                difficulty = 'medium',
                sweetSpotRange = 30,
                sweetSpotSpeed = 0.4,
                pressureSpeed = 3,
            },
            ['hard'] = {
                gameTime = 45000,
                numberOfPins = 5,
                difficulty = 'hard',
                sweetSpotRange = 25,
                sweetSpotSpeed = 0.4,
                pressureSpeed = 2,
            },
}
```

<figure><img src="https://upload.devhub.gg/dh_upload/docs/minigames-1-8.webp" alt=""><figcaption></figcaption></figure>

***

**Game ID**: minigame\_9

```lua
        settings = {
            ['easy'] = {
                targetScore = 5,
                gameTime = 45000,
                penaltyTime = 500,
                wordList = {
                    'cat', 'dog', 'run', 'jump', 'blue', 'red', 'big', 'small', 'fast', 'slow',
                    'hot', 'cold', 'good', 'bad', 'yes', 'no', 'up', 'down', 'left', 'right',
                    'car', 'tree', 'house', 'book', 'pen', 'cup', 'ball', 'fish', 'bird', 'sun'
                },
            },
            ['medium'] = {
                targetScore = 8,
                gameTime = 30000,
                penaltyTime = 750,
                wordList = {
                    'computer', 'keyboard', 'monitor', 'dragon', 'castle', 'bridge', 'mountain', 'ocean',
                    'adventure', 'treasure', 'mystery', 'journey', 'challenge', 'victory', 'failure',
                    'practice', 'perfect', 'amazing', 'wonderful', 'terrible', 'difficult', 'simple',
                    'creative', 'logical', 'magical', 'powerful', 'peaceful', 'dangerous', 'exciting'
                },
            },
            ['hard'] = {
                targetScore = 12,
                gameTime = 25000,
                penaltyTime = 1000,
                wordList = {
                    'extraordinary', 'sophisticated', 'revolutionary', 'incomprehensible', 'unbelievable',
                    'psychological', 'philosophical', 'technological', 'international', 'environmental',
                    'characteristics', 'responsibilities', 'opportunities', 'accomplishments', 'prerequisites',
                    'transformation', 'communication', 'organization', 'imagination', 'determination',
                    'concentration', 'investigation', 'representation', 'consideration', 'demonstration'
                },
            },
}
```

<figure><img src="https://upload.devhub.gg/dh_upload/docs/minigames-1-9.webp" alt=""><figcaption></figcaption></figure>

***

**Game ID**: minigame\_10

```lua
settings = {
            ['easy'] = {
                targetScore = 5,
                gameTime = 30000,
                flashDuration = 1200,
                minDelayBetweenFlashes = 1000,
                maxDelayBetweenFlashes = 2500,
                reactionTimeLimit = 1800,
                penaltyTime = 1000,
            },
            ['medium'] = {
                targetScore = 8,
                gameTime = 35000,
                flashDuration = 1000,
                minDelayBetweenFlashes = 800,
                maxDelayBetweenFlashes = 2200,
                reactionTimeLimit = 1500,
                penaltyTime = 2000,
            },
            ['hard'] = {
                targetScore = 12,
                gameTime = 40000,
                flashDuration = 800,
                minDelayBetweenFlashes = 600,
                maxDelayBetweenFlashes = 1800,
                reactionTimeLimit = 1200,
                penaltyTime = 3000,
            },
        }
 },
```

<figure><img src="https://upload.devhub.gg/dh_upload/docs/minigames-1-10.webp" alt=""><figcaption></figcaption></figure>

***

## <mark style="color:yellow;">**Command Configuration**</mark>

```lua
Shared.CommandEnabled = true
```

* **Description**: Controls whether the test command for minigames is enabled or disabled.
* **Default**: `true`
* **Usage**: When enabled, allows the use of `/startMinigame` command for testing purposes.

***

## <mark style="color:yellow;">**Command Usage**</mark>

When Shared`.CommandEnabled` is set to `true`, you can use the following command:

```
/startMinigame [minigame] [mode]
```

**Parameters:**

* **minigame**: The type of minigame to start
* **mode**: \[easy, medium, hard]

**Examples:**

```
/startMinigame minigame_1 easy
/startMinigame minigame_1 medium
/startMinigame minigame_1 hard
```

***

## <mark style="color:yellow;">**Export Function**</mark>

```lua
local result = exports['devhub_minigames']:startMinigame(minigame, mode, overrideSettings)
```

* **Description**: Programmatically start a minigame from another resource.
* **Parameters**:
  * `minigame` (string): The minigame type to start
  * `mode` (string): Mode
  * overrideSettings (table) If you want to make settings dynamic you can add override settings from config
* **Returns**: Boolean indicating success or failure of the minigame
* **Example Usage**:

```lua
local success = exports['devhub_minigames']:startMinigame("minigame_1", "easy")
if success then
    print("Player successfully completed the minigame!")
else
    print("Player failed the minigame.")
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
