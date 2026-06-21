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

* Download the <mark style="color:red;">WHO AM I</mark> script file from keymaster.&#x20;
* Download the <mark style="color:red;">WHO AM I ASSETS</mark> script file from keymaster.&#x20;
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```javascript
ensure devhub_lib
ensure devhub_whoami
ensure devhub_whoami_assets
```
{% endstep %}

{% step %}
### Restart your server
{% endstep %}
{% endstepper %}

{% hint style="info" %}
For this script **devhub\_lib** is not need.
{% endhint %}

{% content-ref url="../download-purchased-assets.md" %}
[download-purchased-assets.md](../download-purchased-assets.md)
{% endcontent-ref %}
