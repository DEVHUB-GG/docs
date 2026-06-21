# ⚙️ Framework

{% hint style="success" %}
File Path: <kbd>modules/frameworks</kbd>&#x20;
{% endhint %}

***

## <mark style="color:yellow;">Pre configured frameworks</mark>

* esx
* qb
* qbox
* vrp
* custom

***

## <mark style="color:yellow;">Auto detection of resource</mark>

By default, the script will try to automatically detect the correct resource.

```lua
Shared.Framework = "AUTO DETECT"
```

***

## <mark style="color:yellow;">Auto detection failed</mark>

If auto detect failed in <kbd>config.lua</kbd>, set **Shared.Framework** to use the appropriate resource or set it to custom and configurate it yourself.

***

## <mark style="color:yellow;">Custom functionality</mark>

Go to <kbd>modules/frameworks/custom</kbd> and customize your logic
