---
description: >-
  The skill generator and adding your own skills is only available in the
  exclusive version
---

# ⚙️ Skill Generator

{% hint style="danger" %}
Make sure `devhub_skillTree_generator` is started
{% endhint %}

{% stepper %}
{% step %}
### Open configs/sh.main.lua
{% endstep %}

{% step %}
### Change `Config.EnableGenerator` to <mark style="color:green;">**true**</mark>
{% endstep %}

{% step %}
### Restart `devhub_skillTree` script
{% endstep %}

{% step %}
### A new button will appear in the right top corner

<figure><img src="https://upload.devhub.gg/dh_upload/docs/skill-tree-v2-1.webp" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Click at any box on the grid and start creating or editing

<figure><img src="https://upload.devhub.gg/dh_upload/docs/skill-tree-v2-2.webp" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Make sure that not errors are found

<figure><img src="https://upload.devhub.gg/dh_upload/docs/skill-tree-v2-3.webp" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Confirm generation of file

Config will be saved to your <mark style="color:green;">**clipboard**</mark>
{% endstep %}

{% step %}
### Open your `configs/sh.skills.lua` and paste copied skill tree
{% endstep %}

{% step %}
### Create your skill functionality using our export and listeners
{% endstep %}
{% endstepper %}

{% content-ref url="development/" %}
[development](development/)
{% endcontent-ref %}



<figure><img src="https://upload.devhub.gg/dh_upload/docs/skill-tree-v2-4.webp" alt=""><figcaption><p>Showcase of skill tree generator</p></figcaption></figure>
