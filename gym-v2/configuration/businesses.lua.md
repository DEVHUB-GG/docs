---
description: Documentation for businesses.lua Configuration
---

# businesses.lua

Name pools. Nothing else. This file only matters if you run the business module.

```lua
Shared.Business.Delivery = {
    CompanyNames = { "Atlas Logistics", "Pacific Express", "GoldStar Shipping", ... },
    FirstNames   = { "James", "John", "Robert", ... },
    LastNames    = { "Smith", "Johnson", "Williams", ... },
}
```

* **Description**: When a warehouse order is placed, the script invents a delivery company and a driver name for it so the order looks like it came from somewhere. These three lists are what it picks from.
* **CompanyNames**: around 180 fictional logistics companies.
* **FirstNames / LastNames**: around 100 first names and 100 surnames, combined at random.

***

## <mark style="color:yellow;">**Localizing them**</mark>

If your server is not English speaking, replacing these lists is the single easiest bit of flavor you can add. Swap in company names and personal names that fit your city, keep the table structure exactly as it is, and restart the resource.

```lua
Shared.Business.Delivery = {
    CompanyNames = { "Wisla Logistyka", "Bałtyk Transport", "Karpaty Spedycja" },
    FirstNames   = { "Jan", "Piotr", "Anna", "Katarzyna" },
    LastNames    = { "Nowak", "Kowalski", "Wisniewski" },
}
```

{% hint style="info" %}
Every list needs at least one entry. Keeping them long is what stops the same courier turning up on every single delivery.
{% endhint %}
