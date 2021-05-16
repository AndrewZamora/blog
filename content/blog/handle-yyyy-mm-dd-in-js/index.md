---
title: Handle YYYY-MM-DD in JavaScript
date: "2021-05-16T10:41:05.720Z"
description:  Handle YYYY-MM-DD in JavaScript without a library.
---
Getting the year, month and date in JavaScript without a library isn't very difficult. However, getting it into the format you want can be frustrating the first time you encounter this issue. I approach this in two different ways depending on whether I'm using an ISO date string or local date string.

## ISO Date String
```
// Sun May 16 2021 20:01:05 GMT+0900 (Japan Standard Time)
const exampleA = new Date();
const [ year, month, date ] = exampleA.toISOString().split("T")[0].split("-");
// "2021-05-16"
console.log(`${year}-${month}-${date}`);
```

## Local Date String
```
// Sun May 16 2021 20:58:45 GMT+0900 (Japan Standard Time)
const exampleB = new Date();
const [ date, month, year] = exampleB.toLocaleDateString("en-GB").split("/");
// "2021-05-16"
console.log(`${year}-${month}-${date}`);
```
Please note that the date string that the `toLocaleDateString` method returns changes depending on the locale code that is passed to it. For example, `toLocaleDateString("en-US")` will return `"5/16/2021"`. Make sure to adjust how you split and deconstruct the date string to deal with your local date format.

## Adding or Omitting a Leading Zero
Sometimes you will need pass the a date to an input or component with or without a leading zero. The `slice` method and the `parseInt` function can accomplish this. 

Adding a Leading Zero:
```
const [month, date, year] = (new Date()).toLocaleDateString("en-US").split("/");
// "2021-5-5"
console.log(`${year}-${month}-${date}`);
// "2021-05-05"
console.log(`${year}-${("00" + month).slice(-2)}-${("00" + date).slice(-2)}`);
```
Omit a Leading Zero:

```
const [date, month, year] = (new Date()).toLocaleDateString("en-GB").split("/");
// "2021-05-05"
console.log(`${year}-${month}-${date}`);
// "2021-5-5"
console.log(`${year}-${parseInt(month)}-${parseInt(date)}`);
```
## Conclusion
Getting YYYY-MM-DD in your desired format in JavaScript without a library isn't straight forward. But the `split` method and array deconstructing can make the process less difficult when a library isn't an option.




