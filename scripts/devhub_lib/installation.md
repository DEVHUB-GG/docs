# 💻 Installation

{% stepper %}
{% step %}
### Install devhub\_lib

Download and install the required library and configure it according to your framework.

Download [https://github.com/DEVHUB-GG/devhub\_lib ](https://github.com/DEVHUB-GG/devhub_lib)or use command

```bash
git clone https://github.com/DEVHUB-GG/devhub_lib.git
```
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```javascript
ensure devhub_lib
```
{% endstep %}

{% step %}
### Restart your server
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
DO NOT CHANGE RESOURCE NAME
{% endhint %}

{% hint style="info" %}
devhub\_lib is required for other script to work correctly !
{% endhint %}

## <mark style="color:yellow;">Why do i need it ?</mark>

Devhub\_lib is a library for Devhub scripts. It contains all the essential functions that a user might want to edit to ensure the script works properly on their server. It allows you to configure various aspects such as framework, target system, sound system, and more.

It is also open-source, which gives you huge customization possibilities.

Another big advantage is that once you configure devhub\_lib, all future Devhub scripts become plug-and-play, making setup much easier.
