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

In order to get the credentials to work we need to create a client.

```
const translate = new Translate({ credentials: googleCreds });
```

Next, define some text that you want to translate and the target language you want to translate it into.

```
const text = 'Hello, world!';
const target = 'ja'; // Language code for Japanese
```

The Google translate API has a language code for each language they support which is listed [here](https://cloud.google.com/translate/docs/languages).

Create an Immediately Invoked Function Expression (IIFE). So that any code that we wrap it in will be triggered when we call our `index.js` file with `Node`.

```
(async () => {
  // any code in here will be trigger when the node index.js command is ran
})();
```

I have added the keyword `async` to the IIFE so that we can use the `await` keyword to handle returned values from promises. Without this we would need to chain `.then` to any promises that we use.

Above the IIFE create an async function called `translateText` with `text` and `target` as arguments. Then call it in the IIFE.

```
async function translateText(text,target) {

};

(async ()=> {
  await translateText(text, target);
})();
```

In `translateText` function we will use the`translate` method on the client to get a translation.

```
async function translateText(text,target) {
  const translations = await translate.translate(text, target);
  console.log(translations);
  return translations;
};
```

`index.js` should look like this:

```
const googleCreds = require('./google-creds.json');
const { Translate } = require('@google-cloud/translate').v2;

const translate = new Translate({ credentials: googleCreds });

async function translateText(text, target) {
    const translations = await translate.translate(text, target);
    console.log(translations);
    return translations;
};

(async ()=> {
  await translateText(text, target);
})();

```

In your terminal run `node index.js` and you should get this output:

```
[ 'こんにちは世界！', { data: { translations: [Array] } } ]

```

This output is nice but if we just want the translation we will have to access is like this `translations[0]`. Because the output is an Array we can use destructuring to concisely access the translations variable.

```
async function translateText(text, target) {
  const [translations, otherInfo] = await translate.translate(text, target);
      console.log(translations, otherInfo.data);
      // The second item in the array contains details about text that was translated
      return translations;
};
```

Now that we can translate text we might also want to create a function that detects the language of some text.

```
async function detectLanguage(text) {
    const detection = await translate.detect(text);
    console.log(detection);
    return detection;
};

(async ()=> {
  await translateText(text, target);
  await detectLanguage('Hola');
})();
```

Run `node index.js` again and the output of the `detectLanguage` function should be this:

```
[
  { confidence: 0.91015625, language: 'es', input:'Hola' },
  { data: { detections: [Array] } }
]
```

Again, because the returned value is an array we can refactor the `detectLanguage` function using destructuring.

```
async function detectLanguage(text) {
    const [{confidence,language, input}, {data}] = await translate.detect(text);
    console.log(language, confidence, input, data);
    return language;
};
```

The last method to cover is the `getLanguages` method which allows us to programmatically list all languages supported by the Google Translate API. Lets create a function and call the `getLanguages` method.

```
async function getLanguages() {
    const languages = await translate.getLanguages();
    console.log(languages);
    return languages;
};

(async ()=> {
  await translateText(text, target);
  await detectLanguage('Hola');
  await getLanguages();
})();
```
Run `node index.js` and the output for the`getLanguages` function should be this: 

```
[
  [
    { code: 'af', name: 'Afrikaans' },
    { code: 'sq', name: 'Albanian' },
    { code: 'am', name: 'Amharic' },
    { code: 'ar', name: 'Arabic' },
    { code: 'hy', name: 'Armenian' },
    { code: 'az', name: 'Azerbaijani' },
    { code: 'eu', name: 'Basque' },
    { code: 'be', name: 'Belarusian' },
    { code: 'bn', name: 'Bengali' },
    { code: 'bs', name: 'Bosnian' },
    { code: 'bg', name: 'Bulgarian' },
    { code: 'ca', name: 'Catalan' },
    { code: 'ceb', name: 'Cebuano' },
   // The list is longer than this but I have cut it off to save space
  ],
  { data: { languages: [Array] } }
]
```
The output is an array so we can refactor with destructuring for this function too. In addition the `getLanguages` method can take a language code as an argument. This is handy because we might want to list supported languages in another language besides English. Lets account for this argument in our refactor as well. 

```
async function getLanguages(target) {
    const [languages, otherInfo] = await translate.getLanguages(target && target);
    // Instead of an if statement we can also create a conditional with &&. In this situation target && target will evaluate to a value if target is true otherwise it will evaluate to nothing 
    console.log(languages, otherInfo.data);
    return languages;
};

(async () => {
    await translateText(text, target);
    await detectLanguage("Hola");
    await getLanguages();
    await getLanguages("ja");
})();
```
After running `node index.js`, the `getLanguages` function with the argument of `ja` should give the same output as the `getLanguages` without an argument but in Japanese.