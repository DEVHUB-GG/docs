# 🧭 Ui

{% hint style="success" %}
File Path: <kbd>modules/systems/c.ui.lua</kbd>
{% endhint %}

{% hint style="info" %}
Ensure that if custom functionality is utilized, it returns true.
{% endhint %}

{% hint style="warning" %}
Some script may override this position, in that case, you will have a config in a script which will override this value
{% endhint %}

***

## <mark style="color:yellow;">Notification</mark>

### Ui Position

<kbd>top-left</kbd> <kbd>top-right</kbd>  <kbd>bottom-left</kbd>  <kbd>bottom-right</kbd>  <kbd>top-center</kbd>  <kbd>bottom-center</kbd>

```lua
CustomUi.NotifyPosition = "top-right"
```

### Notification Types

```lua
CustomUi.NotifyTypes = {
    ['info'] = "info",
    ['error'] = "error",
    ['success'] = "success",
    ['warning'] = "warning"
}
```

### Custom Notification

<kbd>text</kbd> The text of the notification. \
<kbd>duration</kbd> The duration for which the notification should be displayed. In milliseconds. \
<kbd>notificationType</kbd> The type of the notification. `'info'` `'error'` `'success'` `'warning'`

```lua
CustomUi.Notify = function(text, duration, notificationType)
    return false -- if you are using a custom notification, return true
end
```

***

## <mark style="color:yellow;">Static Message</mark>

### Ui Position

<kbd>top-left</kbd> <kbd>top-right</kbd>  <kbd>bottom-left</kbd>  <kbd>bottom-right</kbd> <kbd>{right = 0, top = 0}</kbd>&#x20;

```lua
CustomUi.StaticMessagePosition = "top-left"
```

### Custom Static Message

<kbd>text</kbd> The text of the static message, if false ui is hidden.

```lua
CustomUi.ShowStaticMessage = function(text)
    return false -- if you are using a custom static message, return true
end
```

***

## <mark style="color:yellow;">Control Buttons</mark>

### Ui Position

<kbd>top-left</kbd> <kbd>top-right</kbd>  <kbd>bottom-left</kbd>  <kbd>bottom-right</kbd> <kbd>{right = 0, top = 0}</kbd>&#x20;

```lua
CustomUi.ControlButtonsPosition= "bottom-right"
```

### Custom Control Buttons

Displays control buttons to the user.

<kbd>text</kbd>  The text associated with the control buttons, if false ui is hidden.

```lua
CustomUi.ShowControlButtons = function(text)
    return false -- if you are using a custom control buttons system, return true
end
```

***

## <mark style="color:yellow;">Progressbar</mark>

### Ui Position

<kbd>low</kbd> <kbd>medium</kbd> <kbd>high</kbd>

```lua
CustomUi.ProgressbarPlacement = "low"
```

### Custom Progressbar

<kbd>data</kbd>  The data for the progress bar.

&#x20;            <kbd>data.duration</kbd>  The duration for which the progress bar should be displayed. In milliseconds.

&#x20;            <kbd>data.text</kbd>  The text associated with the progress bar.

&#x20;            <kbd>data.canStop</kbd>  A boolean indicating whether the progress bar can be stopped by the user.

&#x20;            <kbd>data.placement</kbd>  The placement of the progress bar. Default low. \[low, medium, high]

<kbd>cb</kbd>  The callback function to be executed when the progress bar completes.

```lua
CustomUi.ShowProgressbar = function(data, cb)
    return false -- if you are using a custom progress bar, return true
end
```

***

## <mark style="color:yellow;">Close Progressbar</mark>

Closes the progress bar

```lua
CustomUi.CloseProgressbar = function()
    return false -- if you are using a custom progress bar, return true
end
```
