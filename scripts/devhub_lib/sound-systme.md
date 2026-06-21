# 🔊 Sound System

{% hint style="success" %}
File Path: <kbd>modules/systems/c.sound.lua</kbd>
{% endhint %}

***

{% hint style="info" %}
By default, only xsound is configured.
{% endhint %}

```lua
    Core.PlaySoundLocally = function(uid, url, volume, loop)
        exports['xsound']:PlayUrl(uid, url, volume, loop)
    end
    Core.PlaySoundCoords = function(uid, url, volume, coords)
        exports['xsound']:PlayUrlPos(uid, url, volume, coords, false, false)
    end
    Core.FadeIn = function(uid, time, volume)
        exports['xsound']:fadeIn(uid, time, volume)
    end
    Core.FadeOut = function(uid, time)
        exports['xsound']:fadeOut(uid, time)
    end
    Core.ChangeDistance = function(uid, distance)
        exports['xsound']:Distance(uid, distance)
    end
    Core.StopSound = function(uid)
        exports['xsound']:Destroy(uid)
    end
    Core.SoundExists = function(uid)
        return exports['xsound']:soundExists(uid)
    end
```
