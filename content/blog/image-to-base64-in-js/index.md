---
title: Image to Base64 in JS
date: "2022-04-28T13:54:14.491Z"
description: Get an image base64 string from a file input in JavaScript
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
