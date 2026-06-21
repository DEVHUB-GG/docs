# config.js

```javascript
window.config = {
    numberFormatting: [/\B(?=(\d{3})+(?!\d))/g, " "],
    priceNumberFormatting: [/\B(?=(\d{3})+(?!\d))/g, " "],
    priceFormatting: [/^(\d)/, '$$$1'],
    soundVolume: 0.2,
};
```

## **Price Formatting**

Currency at the end \[/(\d)$/, '$1$'] to change currency symbol \[/(\d)$/, '$1€']

Currency at the beginning \[/^(\d)/, '\$$$1'] to change currency symbol \[/^(\d)/, '€$1']
