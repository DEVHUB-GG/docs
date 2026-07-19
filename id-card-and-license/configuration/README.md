---
description: >-
  Configuration overview for the License System. Each source config file has its
  own page describing every option.
---

# 🛠️ Configuration

The License System splits its configuration across several files in the `configs/` folder. Each
file is documented on its own page:

* [**sh.main.lua**](sh.main.lua.md) — shared gameplay settings: pickup NPC, usage triggers, item
  shops, hideout, fake ID stations and mission, laptop integration.
* [**s.main.lua**](s.main.lua.md) — server-only settings: Discord logging webhook and the license
  data fields.
* [**s.compat.lua**](s.compat.lua.md) — framework compatibility layer: esx\_license impersonation,
  the classic `user_licenses` SQL mirror, and the QBCore/Qbox `metadata.licences` mirror.
* [**s.imagehost.lua**](s.imagehost.lua.md) — where license avatar photos are hosted (uploadhub.gg
  webhook or a custom host). See also the [🖼️ Avatar Photo Hosting](../avatar-photo-hosting.md) guide.
* [**sh.lang.lua**](sh.lang.lua.md) — all translatable text (notifications, NPC dialog, UI labels).

{% hint style="info" %}
The `configs/` files are escrow-ignored (`escrow_ignore` in `fxmanifest.lua`), so you can freely
edit them. The open-function hook files `c.functions.lua` and `s.functions.lua`, plus all client
and server exports, are documented separately under [Exports & Events](../exports-and-events.md).
{% endhint %}
