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

Before we can start writing code, create an account on the Google Cloud Dashboard and generate an API Key. Your API key is tied to your credit card so it needs to be secured. This can be done on the Google Cloud Dashboard by whitelisting the IPs or domains you plan to use the API key with. In addition, the API key should be hidden from version control. One way to do this is with Git by:

Initializing the directory for Git and creating a `.gitignore` and `google-creds.json` file.

```
git init
touch .gitignore
touch google-creds.json

```

Copy and paste your Google API credentials into `google-creds.json`. In the `.gitignore` file list the credentials file like so:

```
google-creds.json
```

If you run `git init` and run `git status` you will notice that every file expect `google-creds.json` is marked as untracked.

```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore
        index.js
        node_modules/
        package-lock.json
        package.json

nothing added to commit but untracked files present (use "git add" to track)
```
Git is ignoring `.google-creds.json`. If our `translate` folder were to be pushed to an online repository, we won't have to worry about exposing this projects API credentials. While we are at it let's also add `/node_modules` to `.gitignore` as well because we don't need to keep track of changes in our dependencies and we can always install them with `npm i`.

Now we can start coding. We will connect `index.js` to `google-creds.json` and the Google Translate code we installed with NPM.
```
const googleCreds = require('./google-creds.json'); 
// If you console.log(googleCreds) you will see your API keys
const { Translate } = require('@google-cloud/translate').v2;
// This connects our code to Googles translate API.

```