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
### Install resources from keymaster

Download the <mark style="color:red;">LICENSE SYSTEM</mark> script file from keymaster.
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```javascript
ensure devhub_lib
ensure devhub_licenses
```

{% hint style="info" %}
On its **first** start the license system automatically sets up its built-in image API (used to host license avatar photos on your server). FiveM's built-in `yarn` resource installs the required Node dependencies for you — this can add a few seconds to the first start and needs **no** manual `npm install`. Following starts are instant.
{% endhint %}
{% endstep %}

{% step %}
### Database Setup

Import the `sql.sql` file into your database.
{% endstep %}

{% step %}
### Restart your server
{% endstep %}
{% endstepper %}



{% content-ref url="../scripts/devhub_lib-needed-for-each-script/" %}
[devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/)
{% endcontent-ref %}
