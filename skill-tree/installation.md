# 💻 Installation

{% stepper %}
{% step %}
#### Install devhub\_lib

Download and install the required library and configure it according to your framework.

Download [https://github.com/DEVHUB-GG/devhub\_lib ](https://github.com/DEVHUB-GG/devhub_lib)or use command

```bash
git clone https://github.com/DEVHUB-GG/devhub_lib.git
```
{% endstep %}

{% step %}
#### Install resources from keymaster

* Download the <mark style="color:red;">SKILL TREE</mark> script file from keymaster.
* If owned download the <mark style="color:red;">SKILL TREE GENERATOR</mark> script file from keymaster.
{% endstep %}

{% step %}
#### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

```bash
ensure devhub_lib        # Must be loaded before devhub_skillTree
ensure devhub_skillTree_generator  # Only if you own exclusive version
ensure devhub_skillTree  # Main resource
```
{% endstep %}

{% step %}
#### Database Setup

Sql will be automatically added, in case you dont see new dh\_skilltree table in your database

Import the `sql.sql` file manualy

{% hint style="info" %}
**Upgrading from v2?** v3 adds two new columns (`data`, `name`) to the `dh_skilltree` table. Run the SQL migration listed in the migration guide so the script can load existing player data.
{% endhint %}
{% endstep %}

{% step %}
#### Restart your server
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
DO NOT CHANGE RESOURCE NAME
{% endhint %}
