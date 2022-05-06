---
title: Scroll to an element in Vue
date: "2021-12-13T10:41:49.267Z"
description: How to scroll to an HTML element in Vue JS with or without an offset.
---

## Introduction

Scrolling to an HTML element in Vue is fairly straightforward but doing so with an offset is not. This guide will discuss how to do both.
_Please note examples below will be using Vue.js version 2._

## Create a scrollable container

```html
<div
  id="app"
  style="height:300vh; background:green; display: flex; flex-direction: column; justify-content: space-between; align-items: center;"
>
  <button style="font-size: 60px; cursor:pointer;" @click="scrollToBottom">
    Scroll to Bottom Element
  </button>
  <h1 ref="bottom">Bottom</h1>
</div>
```

The markup above uses `height: 300vh;` to create a scrollable container. The `h1` element should be below your screens view. Adding `ref="bottom"` will expose the `h1`'s DOM methods that would normally be available if we were not using Vue.

## Trigger scrolling

```javascript
const app = new Vue({
  el: "#app",
  data: {},
  methods: {
    scrollToBottom() {
      this.$refs["bottom"].scrollIntoView({ behavior: "smooth" })
    },
  },
})
```

In the `scrollToBottom` method `this.$refs['THE_ELEMENT_YOU_WANT_TO_TARGET]` instead of `document` and a selector is used to call `scrollIntoView` (Passing `{behavior: "smooth"}` as options allows for smooth scrolling). Clicking the button now will scroll to the bottom element.

https://codepen.io/andrewzamora/pen/BawmWbx

## Adding an Offset

So far scrolling in Vue is pretty simple with the `scrollIntoView` method. The problem is that if there is an element like a nav bar or header that push down the element that you are trying to scroll to, it's going to get cut off. `scrollIntoView` has several options we can pass it but none of them will allow for offsetting the position that it scrolls to. In the code below pressing the "Scroll To Element with No Offset" button demonstrates an offset issue caused by a `header` element.

```html
<header
  style="position: fixed; height: 50px; background: purple;width: 100%;text-align:center"
>
  HEADER
</header>
<div
  id="app"
  style="height:300vh; background:green; display: flex; flex-direction: column; align-items: center;padding-top: 50px;"
>
  <button
    style="font-size: 1em; cursor:pointer; background: red;"
    @click="scrollToElement"
  >
    Scroll to Element with No Offset
  </button>
  <button
    style="font-size: 1em; cursor:pointer; background: green;"
    @click="scrollToHiddenElement"
  >
    Scroll to Element with an Offset
  </button>
  <div style="position: relative">
    <div
      ref="hiddenElement"
      style="height: 50px; position: absolute; top: -50px; pointer-events: none;"
    ></div>
    <h1 ref="element">Element You Want To Scroll To</h1>
  </div>
</div>
```

```javascript
const app = new Vue({
  el: "#app",
  data: {},
  methods: {
    scrollToElement() {
      this.$refs["element"].scrollIntoView({ behavior: "smooth" });
    },
    scrollToHiddenElement() {
      this.$refs["hiddenElement"].scrollIntoView({ behavior: "smooth" });
    }
  }
});
```

https://codepen.io/andrewzamora/pen/ZEXBmGY

One way to handle this issue would be to use Window.[ScrollTo()](https://developer.mozilla.org/en-US/docs/Web/API/Window/scrollTo) instead of `scrollIntoView`. If you have the access to the `Window` object this works well. The code example above is a good workaround if you can't access the `Window` object. The `h1` element is wrapped in a parent element which is positioned `relative`. This allows for hiding a positioned `absolute` element above the `h1`. This "hidden" element's `top` and `height` styles are set to how much of an offset is needed without making the page look visually taller to the user. Then by adding `ref="hiddenElement"` to the "hidden" element, `scrollIntoView` will scroll to it. But to the user, they will be scrolled to their desired element without having it cut off. Clicking the "Scroll to Element With Offset" button above will demonstrate this.

## Conclusion

In Vue.js use a `ref` and`scrollIntoView` to scroll to an HTML Element. But `scrollIntoView` doesn't have options for an offset. Instead use `Window.ScrollTo()` for an offset if you have access to the `Window` Object. If not use a `ref` and `scrollIntoView` to scroll to a positioned absolute element that is the same size as the offset you want.
