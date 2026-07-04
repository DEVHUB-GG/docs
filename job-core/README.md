---
description: >-
  Job Core is a group-based job framework. This documentation covers the
  developer API — the JobAPI table and events you use to build jobs, missions
  and reward flows inside a job copy.
---

# 🧰 Job Core

{% hint style="info" %}
[Tebex Store](https://store.devhub.gg)
{% endhint %}

Job Core is a **group-based job framework**: parties/crews, a shared mission session
with synced objectives and a HUD, a shared payout pool with leader-defined splits,
XP/levels, a progression ladder, quests and boosts — all handled for you.

It's a **copy-per-job template**: copy the resource, give it a unique `Config.JobId`, and
build your job's gameplay inside the copy. Your gameplay drives the core through one global
table — **`JobAPI`** — and a set of **events**, covered below. Every `JobAPI` function takes
the player's server id (`source`); the events tell you when things happen in a run.

{% content-ref url="exports-and-events.md" %}
[exports-and-events.md](exports-and-events.md)
{% endcontent-ref %}

{% hint style="warning" %}
Job Core requires **`devhub_lib`** running on your server. Ensure it before Job Core.
{% endhint %}
