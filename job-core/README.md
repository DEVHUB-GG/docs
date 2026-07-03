---
description: >-
  Job Core is a group-based job framework. This documentation covers the
  developer integration API — the exports and events you use to build jobs,
  missions and reward flows from your own resource.
---

# 🧰 Job Core

{% hint style="info" %}
[Tebex Store](https://store.devhub.gg)
{% endhint %}

Job Core is a **group-based job framework**: parties/crews, a shared mission session
with synced objectives and a HUD, a shared payout pool with leader-defined splits,
XP/levels, a progression ladder, quests and boosts — all handled for you.

You drive it entirely from **your own resource** using the **exports** and **events**
below. You do **not** need access to Job Core's files or config to integrate — every
server export takes the player's server id (`source`) and the events tell you when
things happen in a run.

{% content-ref url="exports-and-events.md" %}
[exports-and-events.md](exports-and-events.md)
{% endcontent-ref %}

{% hint style="warning" %}
Job Core requires **`devhub_lib`** running on your server. Ensure it before Job Core.
{% endhint %}
