---
title: Image to Base64 in JS
date: "2022-04-28T13:54:14.491Z"
description: Get an base64 image string from a file input in JavaScript
---

## Set up file input

```HTML
<html lang="en">
<body>
  <input type="file" id="file-input" />
  <button id="upload-btn">Upload</button>
  <img id="img" style="height: 150px; width: 150px; background: #cfcfcf">
  <p id="indicator">Not Ready to Upload</p>
</body>
</html>
```

Setting the `type` attribute to `file` is all that is needed to get a user to select an image file. The `button`, `p`, and `img` tags are not necessary but will allow for an image preview and to log out the image's base64 string.

## Select DOM Elements

```javascript
const fileInput = document.getElementById("file-input");
const uploadBtn = document.getElementById("upload-btn");
const image = document.getElementById("img");
const indicator = document.getElementById("indicator");
let base64String = "";
```

## Add Event Listeners

```javascript
fileInput.addEventListener("change", fileSelectedHandler);
uploadBtn.addEventListener("click", fileUploadHandler);
```

After the user selects an image file the `change` event will trigger a callback function. The callback function (in this case `fileSelectedHandler`) will be passed an event which will have access to the selected image file.

## Handle Image File in a Callback

```javascript
function fileSelectedHandler(event) {
  if (event.target.files && event.target.files[0]) {
    const FR = new FileReader();
    FR.addEventListener("load", function (e) {
      // An image src can also be set to a Base64 String
      image.src = e.target.result;
      base64String = e.target.result;
      indicator.textContent = "Ready to upload";
    });
    FR.readAsDataURL(event.target.files[0]);
  }
}
```

The `fileSelectedHandler` function checks if a file has been selected (this is a good place to check file types and add error handling). Then the [FileReader](https://developer.mozilla.org/en-US/docs/Web/API/FileReader) instance is created and passed a callback where a base64 string can be retrieved. The `readAsDataUrl` method is called which triggers the load event and its callback.

## Log Out Base64 String

```javascript
function fileUploadHandler() {
  console.log(base64String);
};
```

Clicking on the `Upload` button should log out a base64 string in the developer console and an image preview will appear next to the file input. Also, you can can check out the demo below:

https://codepen.io/andrewzamora/pen/xQQYOQ