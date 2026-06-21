---
description: Documentation for shared.lua Configuration
---

# shared.lua

## <mark style="color:yellow;">**Categories configuration**</mark>

* The `Shared.Words` table contains categories as keys, and each category has a list of words associated with it. Below is an example of the structure:

```lua
Shared = {}
Shared.Words = {
    ["CategoryName"] = {
        "Word1", "Word2", "Word3", -- Add more words as needed
    },
    ["AnotherCategory"] = {
        "WordA", "WordB", "WordC",
    }
}
```

## <mark style="color:yellow;">**Example configuration**</mark>

The default configuration includes the `Animals` category with the following words:

```lua
Shared.Words = {
    ["Animals"] = {
        "Dog", "Cat", "Elephant", "Tiger",
    }
}
```
