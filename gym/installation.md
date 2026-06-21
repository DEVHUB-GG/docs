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
### Install xsound

Download [https://github.com/Xogy/xsound/archive/refs/tags/1.4.3.zip](https://github.com/Xogy/xsound/archive/refs/tags/1.4.3.zip)

_Other sound systems are also possible to use but they will require configuration in devhub\_lib_ [sound-systme.md](../scripts/devhub_lib/sound-systme.md "mention")
{% endstep %}

{% step %}
### Install resources from keymaster

* Download the <mark style="color:red;">GYM</mark> script file from keymaster.&#x20;
{% endstep %}

{% step %}
### Start resources

Move the files to the `resources` folder on your server and add the following lines to your server.cfg in the correct order:

<pre class="language-javascript"><code class="lang-javascript"><strong>ensure xsound
</strong><strong><a data-footnote-ref href="#user-content-fn-1">ensure </a>devhub_lib
</strong>ensure devhub_gym
</code></pre>
{% endstep %}

{% step %}
### Restart your server
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
DO NOT CHANGE RESOURCE NAME
{% endhint %}

{% content-ref url="../scripts/devhub_lib-needed-for-each-script/" %}
[devhub\_lib-needed-for-each-script](../scripts/devhub_lib-needed-for-each-script/)
{% endcontent-ref %}

{% content-ref url="../download-purchased-assets.md" %}
[download-purchased-assets.md](../download-purchased-assets.md)
{% endcontent-ref %}

[^1]: 
