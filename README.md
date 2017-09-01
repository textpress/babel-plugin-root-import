# Babel Root Import
Babel plugin to add the opportunity to use `import` and `require` with root based paths.<br>

## Example
```javascript
// Usually
import SomeExample from '../../../some/example.js';
const OtherExample = require('../../../other/example.js');

// With Babel-Root-Importer
import SomeExample from '~/some/example.js';
const OtherExample = require('~/other/example.js');
```

## Install
**npm**
```
npm install @textpress/babel-plugin-root-import --save-dev
```

**yarn**
```
yarn add @textpress/babel-plugin-root-import --dev
```

## Use
Add a `.babelrc` file and write:
```javascript
{
  "plugins": [
    ["@textpress/babel-plugin-root-import"]
  ]
}

```
or pass the plugin with the plugins-flag on CLI
```
babel-node myfile.js --plugins @textpress/babel-plugin-root-import
```

## Extras
### Custom root-path-suffix
If you want a custom root because for example all your files are in the src/js folder you can define this in your `.babelrc` file
```javascript
{
  "plugins": [
    ["@textpress/babel-plugin-root-import", {
      "rootPathSuffix": "src/js"
    }]
  ]
}
```

### Custom root-path-prefix
If you don't like the `~` syntax you can just use your own symbol (for example a `@` symbol or `\`)
```javascript
{
  "plugins": [
    ["@textpress/babel-plugin-root-import", {
      "rootPathPrefix": "@"
    }]
  ]
}

// Now you can use the plugin like:
import foo from '@/my-file';
```

### Multiple custom prefixes and suffixes
You can supply an array of the above. The plugin will try each prefix/suffix pair in the order they are defined.
```javascript
{
  "plugins": [
    ["@textpress/babel-plugin-root-import", [{
      "rootPathPrefix": "~", // `~` is the default so you can remove this if you want
      "rootPathSuffix": "src/js"
    }, {
      "rootPathPrefix": "@",
      "rootPathSuffix": "other-src/js"
    }, {
      "rootPathPrefix": "#",
      "rootPathSuffix": "../../src/in/parent" // since we suport relative paths you can also go into a parent directory
    }]]
  ]
}

// Now you can use the plugin like:
import foo from '~/my-file';
const bar = require('@/my-file');
```

### Don't let ESLint be confused
If you use [eslint-plugin-import](https://github.com/benmosher/eslint-plugin-import) to validate imports it may be necessary to instruct ESLint to parse root imports. You can use [eslint-import-resolver-babel-root-import](https://github.com/olalonde/eslint-import-resolver-babel-root-import)

```json
    "import/resolver": {
      "@textpress/babel-plugin-root-import": {}
    }
```

### Don't let Flow be confused

If you use Facebook's [Flow](https://flowtype.org/) for type-checking it is necessary to instruct it on how to map your chosen prefix to the root directory. Add the following to your `.flowconfig` file, replacing `~` with your chosen prefix and ensure that the mapping includes the corresponding suffix:
```
[options]
module.name_mapper='^{rootPathPrefix}/\(.*\)$' -> '<PROJECT_ROOT>{rootPathSuffix}/\1'
```

## FYI
Webpack delivers a similar feature, if you just want to prevent end-less import strings you can also define `aliases` in the `resolve` module, at the moment it doesn't support custom/different symbols and multiple/custom suffixes.
[READ MORE](http://xabikos.com/2015/10/03/Webpack-aliases-and-relative-paths/)
