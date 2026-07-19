---
description: Documentation for s.compat.lua Configuration
---

# s.compat.lua

This file controls the **framework compatibility layer**. The License System can impersonate the
classic ESX `esx_license` resource and mirror license state into the QBCore/Qbox
`metadata.licences` table — so third-party scripts written against those systems (esx_dmvschool,
esx_policejob, esx_weaponshop, ox_inventory's license shops, qb-cityhall, qb-policejob, qb-shops
and more) keep working without changing a single line.

Everything in this file is **server-side**. The active framework is detected automatically from the
running resources (`es_extended` / `qb-core` / `qbx_core`); sections that do not apply to your
framework simply stay dormant.

{% hint style="info" %}
The License System stays the **single source of truth**: compatibility state is always derived from
its own licenses and re-asserted after every change and on player login. Changes other scripts make
directly to `user_licenses` or `metadata.licences` are not imported back — issue and revoke
licenses through the License System (exports, issue menu, exams), and the mirrors follow.
{% endhint %}

## <mark style="color:yellow;">**GrantsCountAsLicenses**</mark>

```lua
Config.Compat.GrantsCountAsLicenses = false
```

* **Description**: What counts as "holding" a license for the compatibility layer. With the default
  `false`, a license counts only when the player is the **original owner** of an **active**, non-fake
  license record. Set `true` to also count granted permissions that were never picked up at the clerk
  (a grant means "approved to obtain", the record is the issued card — see
  [Which check do I use?](../exports-and-events.md#which-check-do-i-use)).

***

## <mark style="color:yellow;">**EsxLicense**</mark>

```lua
Config.Compat.EsxLicense = {
    enabled = true,

    typeMap = {
        ["drive"]  = "driving_license",
        ["weapon"] = "weapon_license",
        ["id"]     = "id_card",
        -- ["dmv"]         = "driving_theory",
        -- ["drive_bike"]  = "bike_license",
        -- ["drive_truck"] = "truck_license",
        -- ["boat"]        = "boat_license",
    },

    allowClientAddLicense = false,

    removeLicenseJobs = {
        ["police"] = true,
    },

    sqlMirror = true,
}
```

* **Description**: Full impersonation of the `esx_license` API on top of the License System — the
  `esx_license:addLicense` / `removeLicense` events, the `checkLicense` / `getLicenses` /
  `getLicense` / `getLicensesList` events and their ESX server callbacks. See
  [Framework Compatibility](../exports-and-events.md#framework-compatibility-esx_license--qbcore--qbox)
  for the exact surface other scripts can call.
* **enabled**: Master switch. Even when `true`, the impersonation **auto-disables with a console
  warning** if a real `esx_license` resource is running — <mark style="color:red;">never run
  both at once</mark>.
* **typeMap**: esx\_license license **type** → License System **template name** (the name from the
  license creator, e.g. `driving_license` — not the label). Types not listed fall through unchanged,
  so an esx type that literally matches a template name needs no entry. Uncomment/extend the examples
  to cover every type your other scripts use (`dmv`, `drive_bike`, `drive_truck`, `boat`, …) — an
  unmapped type with no matching template is rejected with a console hint telling you what to add.
* **allowClientAddLicense**: Whether `esx_license:addLicense` may be triggered **directly by a
  client**. The original resource allowed this — it is an exploit vector (cheaters can self-grant
  any license), so it ships `false`. Server-to-server calls (esx\_dmvschool, esx\_weaponshop, …) are
  always allowed and unaffected.
* **removeLicenseJobs**: Jobs allowed to trigger `esx_license:removeLicense` **from a client**
  (esx\_policejob revokes licenses this way). Mirrors the original resource's `Config.allowedJobs`.
  Server-to-server calls bypass this check.
* **sqlMirror**: Maintain the classic `licenses` / `user_licenses` SQL tables (created automatically
  when missing) so scripts that read them **directly** keep working — ox\_inventory's ESX bridge and
  several car dealers run their own `SELECT` on `user_licenses`. Rows stay in sync with active
  licenses; rows of license types unknown to the License System are left untouched. Only active on
  ESX servers.

***

## <mark style="color:yellow;">**QbLicences**</mark>

```lua
Config.Compat.QbLicences = {
    enabled = true,

    map = {
        ["driving_license"] = "driver",
        ["weapon_license"]  = "weapon",
        ["id_card"]         = "id",
        -- ["business_license"] = "business",
    },
}
```

* **Description**: Keeps the QBCore/Qbox `PlayerData.metadata.licences` table in sync with the
  License System, so stock consumers (qb-cityhall, qb-policejob, qb-shops, qb-phone, ox\_inventory's
  QB bridge, …) see the right flags. Keys are re-asserted on every license change **and** on player
  login — which also self-heals the stock qb-policejob seizure handler that wipes custom keys from
  the licences table. Only active on QBCore/Qbox servers.
* **enabled**: Master switch for the metadata mirror.
* **map**: License System **template name** → key inside `metadata.licences`. Multiple templates may
  point at the same key (the key is `true` when **any** of them is held). QBCore ships
  `driver`/`business`/`weapon` by default, Qbox ships `id`/`driver`/`weapon` — unknown keys are fine,
  they are just extra entries. Map entries whose template does not exist on your server are skipped
  entirely (nothing is written for them).

{% hint style="warning" %}
The metadata mirror is **one-way** (License System → `metadata.licences`). A script flipping a
metadata flag directly does **not** create or revoke a license — other resources routinely replace
the whole licences table (stock qb-policejob does), so the metadata cannot be trusted as a source.
Issue licenses through the License System and let the mirror follow.
{% endhint %}

***

## Related Pages

{% content-ref url="../exports-and-events.md" %}
[exports-and-events.md](../exports-and-events.md)
{% endcontent-ref %}
