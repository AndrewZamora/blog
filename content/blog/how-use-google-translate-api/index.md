---
title: The Google Translate API With Node.js
date: "2020-05-16T22:40:32.169Z"
description: Learn how to use the Google Translate API with Node.js.
---

Before we start you are going to need to download and install Node.js and a code editor. Next create a directory named `translate` with an `index.js` file in it.

```
mkdir translate

touch index.js 
```

Change directories into `translate` and open `index.js` in the code editor of your choice. If you are using Visual Studio Code you can use the `code` command to open up the directory.

```
cd translate

code .
```
Because we installed Node.js we can create a `package.json` file with the `npm init` command. If you are not sure what that is, it's a file that helps us manage other developers code that we depend on to have our code run. I'm also going to use the `-y` flag to say yes to all of questions that it prompts you with.

```
npm init -y
```

You should see a package.json file in addition to the `index.js` file in the `translate` directory. After that we can install Google Translate's dependencies with `npm i <NAME OF DEPENDENCE>` command (`i` is short for install).

```
npm i @google-cloud/translate
```
Open up `package.json` in your editor and it should look something like this.

```
{
  "name": "translate-expenses",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@google-cloud/translate": "^5.3.0",
  }
}
```
Before we can start writing code, we need to get an API key from the Google Cloud Dashboard.

