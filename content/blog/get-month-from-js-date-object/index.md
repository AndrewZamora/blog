---
title: Get the month from JS Date Object
date: "2022-01-24T12:55:01.863Z"
description: Get the name of the month in JS without hardcoding an Array
---

JavaScript's Date Object provides the `toLocaleDateString` method. The name of the month is returned by passing it a `locales` string and `options` object as parameters.

```javascript
const dateObj = new Date()
// Set locales to desired location/language format
const locales = "en-US"
// The month property can also be set to "short"
const options = { month: "long" }
// Call method with parameters
const nameOfTheMonth = dateObj.toLocaleDateString(locales, options)
// Log/print the name of the month
console.log(nameOfTheMonth)
```

Click [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString) for browser compatibility and more info about the toLocaleDateString method.
