---
title: Get 12 hour time from JS Date Object
date: "2023-03-13T13:29:17.494Z"
description: Get 12 hour am/pm time from a JavaScript Date Object
---

```javascript
// Create Date Object
const date = new Date();
// Pass options to toLocaleString method
const time = date.toLocaleString('en-US', { hour: 'numeric', minute:'numeric', hours12: true });
// The log should look something like this '10:29 PM'
console.log(time)
```
