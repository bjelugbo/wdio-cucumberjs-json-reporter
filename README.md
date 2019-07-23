# wdio-cucumberjs-json-reporter
A WDIO reporter that creates CucumberJS JSON files for WebdriverIO V5+

> **THIS MODULE CAN ONLY WORK WITH WebdriverIO V5+!

> **THIS MODULE IS NOT A REPLACEMENT OF [wdio-multiple-cucumber-html-reporter](https://github.com/wswebcreation/wdio-multiple-cucumber-html-reporter). THAT MODULE ONLY SUPPORTS WEBDRIVERIO V4 AND ALSO CREATES A REPORT**

## What does it do
This reporter will generate a **Cucumber JSON file** for each feature that is being tested. The JSON file can be used with whatever report you want to use like for example [multiple-cucumber-html-reporter](https://github.com/wswebcreation/multiple-cucumber-html-reporter).

It will also add metadata about the running instance to the feature file and last but not least, it will give you  the opportunity to add attachments to the JSON output.

## Installation
The easiest way is to keep `wdio-cucumberjs-json-reporter` as a devDependency in your `package.json`.

```json
{
  "devDependencies": {
    "wdio-cucumberjs-json-reporter": "^1.0.0"
  }
}
```

You can simple do it by:

```bash
npm install wdio-cucumberjs-json-reporter --save-dev
```

so it will be added automatically to your `package.json`

Instructions on how to install `WebdriverIO` can be found [here](http://webdriver.io/guide/getstarted/install.html).

## Configuration
Configure the output directory in your wdio.conf.js file:

```js
exports.config = {
    // ...
    reporters: [
        // Like this with the default options, see the options below
        'cucumberjs-json',
        
        // OR like this if you want to set the folder
        [ 'cucumberjs-json', { 
                jsonFolder: '.tmp/new/',
            },
        ],
    ],
  // ...
}
```

> DON'T USE BOTH WAYS OF ADDING THE REPORTER, THIS IS JUST AN EXAMPLE!

## Options
### `jsonFolder`
- **Type:** `String`
- **Mandatory:** No
- **Default:** `.tmp/json/`

The directory where the JSON files, generated by this report, will be stored, relative from where the script is started.

**N.B.:** If you use a npm script from the command line, like for example `npm run test` the `jsonFolder` will be relative from the path
where the script is executed. Executing it from the root of your project will also create the `jsonFolder` in the root of you project.

## Metadata
As said, this report can automatically store the metadata of the current machine / device the feature has been executed on. 

To customize this you can add this by adding the following object to your `capabilities`;

```js
// Example wdio.conf.js
exports.config = {
    //..
    capabilities: [
        {
            browserName: 'chrome',
            // Add this
            'cjson:metadata': {
                // For a browser
                browser: {
                    name: 'chrome',
                    version: '58',
                },
                // for an app
                app: {
                  name: 'name.of.app.ipa',
                  version: '1.2.3', 
                },
                device: 'MacBook Pro 15',
                platform: {
                    name: 'OSX',
                    version: '10.12.6'
                }
            },
        },
    ],
};
```

### Metadata values
#### `metadata.app.name`
- **Type:** `string`

**e.g.:** The name of the app.

#### `metadata.app.version`
- **Type:** `string`

**e.g.:** The version of the app.

#### `metadata.browser.name`
- **Type:** `string`
- **Possible values:** `internet explorer | edge | chrome | firefox | safari`

#### `metadata.browser.version`
- **Type:** `string`

**e.g.:** The version of the browser, this can be added manual or retrieved during the execution of the tests to get the exact version number.

#### `metadata.device`
- **Type:** `string`

**e.g.:** A name that represents the type of device. For example, if you run it on a virtual machine, you can place it here `Virtual Machine`,
or the name of the mobile, like for example `iPhone 7 Plus`.

#### `metadata.platform.name`
- **Type:** `string`
- **Possible values:** `windows | osx | linux | ubuntu | android | ios`

#### `metadata.platform.version`
- **Type:** `string`

**e.g.:** The version of the platform

> If you don't provide the `browser`-object in the metadata, this module will automatically determine it for you. **It will always override it with the most recent value it can determine.**

> If you don't provide the `device` and or the `platform`-object it will be defaulted for you to `not known`

> If you don't provide a `browser.name` or a `browser.version` the module will try to determine this automatically.

## Attachment
You have the option to attach data to the JSON file in all these hooks / steps:

- Before(All)
- After(All)
- Given
- When
- Then
- And

The only thing you need to provide is the following code in your step files.

```js
import cucumberJson from 'wdio-cucumberjs-json-reporter';

// Attach a string (if no type is provided it will automatically default to `text/plain`
cucumberJson.attach('just a string');
cucumberJson.attach('just a second string', 'text/plain');

// Attach JSON
cucumberJson.attach({"json-string": true}, 'application/json');

// Attach a screenshot in a before hook
cucumberJson.attach(browser.takeScreenshot(), 'image/png');
