# CliApp
Просто консольная утилита
## Установка

### Из исходников

1. Установить докер
2. Склонировать репозиторий
3. Из директории склонированного репозитория запустить команду

```sh
docker build -t <имя образа> .
```

### Из репозитория Docker Hub
```sh
docker push acglearn/cli:latest 
```

## Config

Size Limits supports three ways to define config.

1. `size-limit` section in `package.json`:

   ```json
     "size-limit": [
       {
         "path": "index.js",
         "import": "{ createStore }",
         "limit": "500 ms"
       }
     ]
   ```

2. or a separate `.size-limit.json` config file:

   ```js
   [
     {
       "path": "index.js",
       "import": "{ createStore }",
       "limit": "500 ms"
     }
   ]
   ```

3. or a more flexible `.size-limit.js` or `.size-limit.cjs` config file:

   ```js
   module.exports = [
     {
       path: "index.js",
       import: "{ createStore }",
       limit: "500 ms"
     }
   ]
   ```

Each section in the config can have these options:

* **path**: relative paths to files. The only mandatory option.
  It could be a path `"index.js"`, a [pattern] `"dist/app-*.js"`
  or an array `["index.js", "dist/app-*.js", "!dist/app-exclude.js"]`.
* **import**: partial import to test tree-shaking. It could be `"{ lib }"`
  to test `import { lib } from 'lib'` or `{ "a.js": "{ a }", "b.js": "{ b }" }`
  to test multiple files.
* **limit**: size or time limit for files from the `path` option. It should be
  a string with a number and unit, separated by a space.
  Format: `100 B`, `10 kB`, `500 ms`, `1 s`.
* **name**: the name of the current section. It will only be useful
  if you have multiple sections.
* **entry**: when using a custom webpack config, a webpack entry could be given.
  It could be a string or an array of strings.
  By default, the total size of all entry points will be checked.
* **webpack**: with `false` it will disable webpack.
* **running**: with `false` it will disable calculating running time.
* **gzip**: with `false` it will disable gzip compression.
* **brotli**: with `true` it will use brotli compression and disable gzip compression.
* **config**: a path to a custom webpack config.
* **ignore**: an array of files and dependencies to exclude from
  the project size calculation.
* **modifyWebpackConfig**: (.size-limit.js only) function that can be used to 
  do last-minute changes to the webpack config, like adding a plugin.

If you use Size Limit to track the size of CSS files, make sure to set
`webpack: false`. Otherwise, you will get wrong numbers, because webpack
inserts `style-loader` runtime (≈2 kB) into the bundle.

[pattern]: https://github.com/sindresorhus/globby#globbing-patterns


## Plugins and Presets

Plugins:

* `@size-limit/file` checks the size of files with Gzip, Brotli
  or without compression.
* `@size-limit/webpack` adds your library to empty webpack project
  and prepares bundle file for `file` plugin.
* `@size-limit/time` uses headless Chrome to track time to execute JS.
* `@size-limit/dual-publish` compiles files to ES modules with [`dual-publish`]
  to check size after tree-shaking.

Plugin presets:

* `@size-limit/preset-app` contains `file` and `time` plugins.
* `@size-limit/preset-big-lib` contains `webpack`, `file`, and `time` plugins.
* `@size-limit/preset-small-lib` contains `webpack` and `file` plugins.

[`dual-publish`]: https://github.com/ai/dual-publish


## JS API

```js
const sizeLimit = require('size-limit')
const filePlugin = require('@size-limit/file')
const webpackPlugin = require('@size-limit/webpack')

sizeLimit([filePlugin, webpackPlugin], [filePath]).then(result => {
  result //=> { size: 12480 }
})
```
