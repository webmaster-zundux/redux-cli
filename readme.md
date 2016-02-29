[![Build Status](https://travis-ci.org/SpencerCDixon/redux-cli.svg?branch=master)](https://travis-ci.org/SpencerCDixon/redux-cli)
[![Code Climate](https://codeclimate.com/github/SpencerCDixon/redux-cli/badges/gpa.svg)](https://codeclimate.com/github/SpencerCDixon/redux-cli)
[![codecov.io](https://codecov.io/github/SpencerCDixon/redux-cli/coverage.svg?branch=master)](https://codecov.io/github/SpencerCDixon/redux-cli?branch=master)
<a href="https://zenhub.io"><img src="https://raw.githubusercontent.com/ZenHubIO/support/master/zenhub-badge.png"></a>

```
______         _                   _____  _     _____  
| ___ \       | |                 /  __ \| |   |_   _|
| |_/ /___  __| |_   ___  ________| /  \/| |     | |   
|    // _ \/ _` | | | \ \/ /______| |    | |     | |   
| |\ \  __/ (_| | |_| |>  <       | \__/\| |_____| |_  
\_| \_\___|\__,_|\__,_/_/\_\       \____/\_____/\___/  
```

## Quick Start

```javascript
npm i redux-cli -g       // install redux-cli globally
redux new <project name> // create a new redux project
redux init               // OR configure a current project to use the CLI

// Start generating components/tests and save time \(• ◡ •)/
//(g is alias for generate)
redux g dumb SimpleButton
```

## Table Of Contents

1.  [Getting Started](#getting-started)
2.  [Configuring Existing Project](#config-existing-project)
3.  [Commands](#commands)
4.  [Generators](#generators)
5.  [Roadmap](#roadmap)
6.  [Examples](#examples)
6.  [Creating Custom Blueprints](#creating-blueprints)
7.  [Issues/Contributing](#contributing)
8.  [Changelog](#changelog)

### Getting Started
Running `redux new <project name>` will pull down the amazing [Redux Starter Kit](https://github.com/davezuko/react-redux-starter-kit) and
initialize a new git repo.  Running `new` will automatically set up a `.reduxrc`
to work with this specific starter kit.  If you want to integrate the CLI in an
existing project or store your components in different pathes please see [config existing project](#config-existing-project)

### Config Existing Project
There is an `init` subcommand for you to specify all pathes to where components
live in your project.  The `init` command just creates a `.reduxrc` in your
project root.  If you want to you can just create the `.reduxrc` manually.


Final `.reduxrc` might look like this:

```javascript
{
  "sourceBase":"src",
  "testBase":"./test",
  "smartPath":"containers",
  "dumbPath":"components",
  "formPath":"components/forms",
  "duckPath":"redux/modules",
  "reducerPath":"",
  "fileExtension":"js",
  "fileCasing": "default",
  "wrapFilesInFolders": false
}
```  

### Initial Configuration
|Key Name|Description|Extra Info|Required|
|---|---|---|---|
|**sourceBase**|where you keep your pre-compiled source|usually going to be `./src`|✓|
|**testBase**|where you keep your tests|usually going to be `./test` or `./tests`  |✓|
|**smartPath**|path where you keep smart/container components|relative from `sourceBase`|✓|
|**dumbPath**|path where you keep your dumb/pure components|realtive from `sourceBase`|✓|
|**formPath**|path where you keep your form components|realtive from `sourceBase`. Assumes you're using redux-form| |
|**duckPath**|path where you keep your Redux Ducks|realtive from `sourceBase`.| |
|**reducerPath**|path where you keep your reducers|realtive from `sourceBase`.| |
|**fileExtension**|do you use .js or .jsx for your React components| |✓|
|**fileCasing**|how do you want generated files to be named (pasal/camel/snake/default)| |✓|
|**wrapFilesInFolders**|Would you like your generated files wrapped in a folder? (true/false)| | |


### Commands

|Command|Description|Alias|
|---|---|---|
|`redux new <project name>`|creates a new redux project||
|`redux init`|configure an existing redux app to use the CLI||
|`redux generate <generator name> `|generates files and tests for you automatically|`redux g`|


### Generators

**Note**: All component names can be passed as `PascalCase`, `snake_case`, `dash-names`,
or `camelCase` and they will be converted to Pascal Case in the generated files.

|Name|Description|Options|
|---|---|---|
|`redux g dumb <comp name> [html tag]`|generates a dumb component and test file|html tag can be passed to prepopulate the render|
|`redux g smart <smart name> [html tag]`|generates a smart connected component and test file|html tag can be passed to prepopulate the render|
|`redux g form <form name>`|generates a form component (assumes redux-form)||
|`redux g duck <duck name>`|generates a redux duck and test file||

### Eamples
Below are some examples of using the generator to speed up development:

```
// generates a dumb component with <button></button> in render
redux g dumb SimpleButton button  

// generates a smart component with <div> in render
redux g smart CommentContainer div

// generate a redux-form with <form> tags in render statement
redux g form ContactForm

// generate a Redux 'duck' (reducer, constants, action creators)
redux g duck todos
```

### Creating Blueprints
Blueprints are template generators with optional custom install logic.  

`redux generate` comes with a default set of blueprints.  Every project has
their own configuration & needs, therefore blueprints have been made easy to override
and extend.

**Preliminary steps**:

1.  Create a `blueprints` folder in your root directory.  Redux CLI will search
for blueprints there _first_ before generating blueprints that come by default.  
2.  Create a sub directory inside `blueprints` for the new blueprint OR use the
blueprint generator (super meta I know) that comes with Redux CLI by typing:
`redux g blueprint <new blueprint name>`.  
3.  If you created the directory yourself than make sure to create a `index.js`
file that exports your blueprint and a `files` folder with what you want
generated.  

**Customizing the blueprint**:

Blueprints follow a simple structure. Let's take the built-in
`smart` blueprint as an example:

```
blueprints/smart
├── files
│   ├── __root__
│   │   └── __name__.js
│   └── __test__
│       └── __name__.test.js
└── index.js
```

#### File Tokens

`files` contains templates for all the files to be generated into your project.

The `__name__` token is subtituted with the 
entity name at install time.  Entity names can be configued in
either PascalCase, snake_case, or camelCase so teams can customize their file
names accordingly.  By default, the `__name__` will return whatever is entered
in the generate CLI command.

For example, when the user
invokes `redux g smart commentContainer` then `__name__` becomes
`commentContainer`.

The `__root__` token is subsituted with the absolute path to your source.
Whatever path is in your `.reduxrc`'s `sourceBase` will be used here.

The `__test__` token is substitued with the absolute path to your tests.
Whatever path is in your `.reduxrc`'s `testBase` will be used here.

The `__path__` token is substituted with the blueprint
name at install time. For example, when the user invokes
`redux generate smart foo` then `__path__` becomes
`smart`.  

#### Template Variables (AKA Locals)

Variables can be inserted into templates with
`<%= someVariableName %>`.  The blueprints use [EJS](http://www.embeddedjs.com/)
for their template rendering so feel free to use any functionality that EJS
supports.

For example, the built-in `dumb` blueprint
`files/__root__/__name__.js` looks like this:

```js
import React, { Component, PropTypes } from 'react';

const propTypes = {
};

class <%= pascalEntityName %> extends Component {
  render() {
    return (
    )
  }
}

<%= pascalEntityName %>.propTypes = propTypes;
export default <%= pascalEntityName %>;
```

`<%= pascalEntityName %>` is replaced with the real
value at install time.  If we were to type: `redux g dumb simple-button` all
instances of `<%= pascalEntityName %>` would be converted to: `SimpleButton`.

The following template variables are provided by default:

- `pascalEntityName`
- `camelEntityName`
- `snakeEntityName`

The mechanism for providing custom template variables is
described below.

#### Index.js

Custom installation (and soon uninstallation) behaviour can be added
by overriding the hooks documented below. `index.js` should
export a plain object, which will extend the prototype of the
`Blueprint` class. 

```js
module.exports = {
  locals: function(options) {
    // Return custom template variables here.
    return {};
  },

  fileMapTokens: function(options) (
    // Return custom tokens to be replaced in your files
    return {
      __token__: function(options){
        // logic to determine value goes here
        return 'value';
      }
    }
  },

  filesPath: function() {
    // if you want to store generated files in a folder named
    // something other than 'files' you can override this
    return path.join(this.path, 'files');
  },

  // before and after install hooks
  beforeInstall: function(options) {},
  afterInstall: function(options) {},
};
```

#### Blueprint Hooks

As shown above, the following hooks are available to
blueprint authors:

- `locals`
- `fileMapTokens`
- `filesPath`
- `beforeInstall`
- `afterInstall`

#### locals

Use `locals` to add custom tempate variables. The method
receives one argument: `options`. Options is an object
containing general and entity-specific options.

When the following is called on the command line:

```sh
redux g dumb foo --type=array --dry-run
```

The object passed to `locals` looks like this:

```js
{
  entity: {
    name: 'foo',
    options: {
      type: 'array'
    }
  },
  dryRun: true
}
```

This hook must return an object. It will be merged with the
aforementioned default locals.

#### fileMapTokens

Use `fileMapTokens` to add custom fileMap tokens for use
in the `mapFile` method. The hook must return an object in the
following pattern:

```js
{
  __token__: function(options){
    // logic to determine value goes here
    return 'value';
  }
}
```

It will be merged with the default `fileMapTokens`, and can be used
to override any of the default tokens.

Tokens are used in the files folder (see `files`), and get replaced with
values when the `mapFile` method is called.

#### filesPath

Use `filesPath` to define where the blueprint files to install are located.
This can be used to customize which set of files to install based on options
or environmental variables. It defaults to the `files` directory within the
blueprint's folder.

#### beforeInstall & afterInstall

Called before any of the template files are processed and receives
the the `options` and `locals` hashes as parameters. Typically used for
validating any additional command line options or for any asynchronous
setup that is needed.   


### Roadmap
- [x] template overriding so people can customize templates
- [ ] more robust blueprint options
- [ ] uninstall hooks for blueprints
- [ ] support for fields option in form generator
- [ ] support for routing (generates both view and route and adds to routes)
- [ ] multiple starter kit support

### Contributing
This CLI is very much in the beginning phases and I would love to have people
help me to make it more robust.  Currently, it's pretty opinonated to use the
tooling/templates I prefer in my projects but I'm open to PR's to make it more
universal and support other platforms (I'm on Mac).

This repo uses Zenhub for managing issues.  If you want to see what's currently
being worked on or in the pipeline make sure to install the [Zenhub Chrome
Extension](https://chrome.google.com/webstore/detail/zenhub-for-github/ogcgkffhplmphkaahpmffcafajaocjbd?hl=en-US)
and check out this projects 'Boards'.

#### Development Setup/Contributing
Use `npm link` is to install the CLI locally when testing it and adding
features.

```
nvm use 5.1.0    // install node V5.1 if not present (nvm install 5.1.0)
npm install
npm i eslint babel-eslint -g  // make sure you have eslint installed globally
npm start        // to compile src into lib
npm test         // make sure all tests are passing

// to test the cli in the local directory you can:
npm link         // will install the npm package locally so you can run 'redux <commands>'
redux <run commands here>
```

### Package Utility Scripts:  
```
npm start        // will watch files in src and compile using babel
npm test         // runs test suite with linting.  Throws when lint failing
npm run lint     // lints all files in src and test
```

### Changelog

`1.3.0` - major internal refactor, addition of customizable blueprints
`1.1.1` - adds support for html tag in render when generating components  
`1.0.1` - adds fileCasing to generators so Linux users can use snake_case_file_names  
`1.0` - first public release with stable api (new/generate/init)  
