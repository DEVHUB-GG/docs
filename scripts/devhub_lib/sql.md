# 📡 SQL

{% hint style="success" %}
File Path: <kbd>modules/systems/s.sql.lua</kbd>
{% endhint %}

***

{% hint style="info" %}
By default, only **oxmysql** is configured.
{% endhint %}

## <mark style="color:yellow;">Async</mark>

```lua
Core.SQL.Execute = function(query, args, cb)
    MySQL.query(query, args, cb)
end
```

## <mark style="color:yellow;">Sync</mark>

```lua
Core.SQL.AwaitExecute = function(query, args)
    return MySQL.query.await(query, args)
    -- RETURN return data in case query is not SELECT
    -- {"warningStatus":0,"info":"","changedRows":0,"serverStatus":2,"fieldCount":0,"insertId":0,"affectedRows":1}
end
```
