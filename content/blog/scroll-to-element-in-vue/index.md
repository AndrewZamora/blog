---
title: Scroll to an element in Vue
date: "2021-12-13T10:41:49.267Z"
description: How to scroll to an HTML element in Vue JS with or without an offset.
---

## Introduction

Scrolling to an HTML in Vue is fairly straightforward but doing so with an offset is not. This guide will discuss how to do both.
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

## Realizing when you need an offset

So far scrolling in Vue is pretty simple with the `scrollIntoView` method. The only problem here is that if there is an element like a nav bar or header that push the element you are trying to scroll to down, it's going to get cut off. `scrollIntoView` has several options we can pass it but none of them will allow for offsetting the position that it scrolls to. The code below demonstrates an offset issue caused by a `header` element.

```html
<header
  style="position: fixed; height: 50px; background: purple;width: 100%;text-align:center"
>
  HEADER
</header>
<div
  id="app"
  style="height:300vh; background:green; display: flex; flex-direction: column; align-items: center;padding-top: 50px"
>
  <button style="font-size: 60px; cursor:pointer;" @click="scrollToBottom">
    Scroll to Bottom Element
  </button>
  <h1 ref="bottom">Bottom</h1>
</div>
```
A header has been added and the `app` container 

```html
<div
  :ref=""
  style="height: 55px; position: absolute; top: -55px; pointer-events: none;"
></div>
```
