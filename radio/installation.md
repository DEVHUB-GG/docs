# 💻 Installation

{% stepper %}
{% step %}
### Install [devhub\_lib](https://docs.devhub.gg/scripts/devhub_lib-needed-for-each-script)

Download and install the required library and [configure](https://docs.devhub.gg/scripts/devhub_lib-needed-for-each-script) it according to your framework.

Download [https://github.com/DEVHUB-GG/devhub\_lib ](https://github.com/DEVHUB-GG/devhub_lib)or use command

```bash
git clone https://github.com/DEVHUB-GG/devhub_lib.git
```
{% endstep %}

{% step %}
### Install resources from keymaster

* Download the <mark style="color:red;">RADIO</mark> script file from keymaster.&#x20;
{% endstep %}

{% step %}
### Add export to pma-voice

[#required-steps-before-use](installation.md#required-steps-before-use "mention")
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```javascript
ensure devhub_lib
ensure devhub_radio
```
{% endstep %}

{% step %}
### Restart your server
{% endstep %}
{% endstepper %}

## <mark style="color:red;">⚠️ REQUIRED STEPS BEFORE USE</mark>

<mark style="color:red;">**1. pma-voice Configuration in server.cfg**</mark>

Make sure that `voice_enableRadioAnim` in `server.cfg` is set to `0`. If it doesn't exist, add to `server.cfg`:

```
setr voice_enableRadioAnim 0
```

<mark style="color:red;">**2. pma-voice Export**</mark>

Add this to the bottom of `pma-voice/client/module/radio.lua`:

```lua
exports("getRadioData", function()
    return radioData
end)
```

{% hint style="danger" %}
DO NOT CHANGE RESOURCE NAME
{% endhint %}

{% content-ref url="../scripts/devhub_lib-needed-for-each-script/" %}
[devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/)
{% endcontent-ref %}

{% content-ref url="../download-purchased-assets.md" %}
[download-purchased-assets.md](../download-purchased-assets.md)
{% endcontent-ref %}
