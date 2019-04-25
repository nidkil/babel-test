# Babel Test

[Babel](https://babeljs.io/) is a toolchain that is mainly used to convert ECMAScript 2015+ code into a backwards compatible version of JavaScript in current and older browsers or environments.

This is a test project to investigate how to setup Babel. It also includes instructions for Webstorm.

## Project structure

```
babel-test
|-- dist
|-- node_modules
|-- src
|   |--index.js
|   |--test.js
|-- .babelrc
|-- package.json
```

## Install

Install the following packages to get Babel to work with Node.
 
```bash
$ npm install --save-dev @babel/core @babel/cli @babel/node @babel/preset-env @babel/register 
```

For easy development we will also add the [nodemon](https://github.com/remy/nodemon) package, which simplifies development by reloading node automatically when a source file changes.

```bash
$ npm install --save-dev nodemon
```

## Create .babelrc file

Create a .babelrc config in the project root and enable some plugins. To start, you can use the env preset, which enables transforms for ES2015+:  In order to enable the preset you have to add it to the `.babelrc` file, like this:

```json
{
  "presets": ["@babel/preset-env"]
}
```

See [here](https://babeljs.io/docs/en/configuration) for more information on it.

## Add script to package.json file

Add some scripts to the package.json to run and build the project with Babel.

```json
"scripts": {
    "clean": "rm -rf dist",
    "build": "npm run clean && babel src --out-dir dist --source-maps",
    "serve:dev": "nodemon --exec babel-node src/index",
    "serve:prd": "node dist/index"
}
```

From the command line you can build the project with:

```bash
$ npm run build
```

And run it in development mode with:

```bash
$ npm run serve:dev
```

## Webstorm

Okay lets get Webstorm working with Babel, so that we can debug our source code.

### Enable EMCAScript 6

Make sure ECMAScript 6 is set as the JavaScript version in WebStorm’s Preferences

```
File > Settings... (Ctrl+Alt+s) > Languages and Frameworks > JavaScript
```

Make sure the `JavaScript Language Version` is set to `ECMAScript 6`.

### Add the Babel file watcher

File watcher is a WebStorm built-in tool that allows you to automatically run commandline tools on file changes. For Babel Webstorm has pre-configured File watchers.

Go to the file watcher settings:

```
File > Settings... (Ctrl+Alt+s) > Tools > File watchers
```

Click the `+` button and select Babel from the list.
 
In the File watcher configuration dialog check if the path to Babel is correct: it should be located in the project node_modules folder, e.g. node_modules/.bin/babel.

All other Watcher settings are predefined and should be ready to go. With this default setup, compiled files will be located in the dist folder.

### Debugging

#### Using @babel/register

When you’re developing a Node.js application in ES6, one of the ways to run and test it is using `@babel/register`.

- In your Node.js run/debug configuration add `-r @babel/register` in the Node parameters field. Don’t forget to specify the path to the JavaScript file you’d like to run.
- Save configuration and hit run or debug.

#### Using @babel/node

Alternatively you can use @babel/node. To use it, you need to install it and setup a script in the `package.json` file. See the section `Add script to package.json file`.
