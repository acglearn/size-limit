# CliApp
Просто консольная утилита
## Установка

### Из исходников

1. Установить Docker
2. Склонировать репозиторий
3. Из директории склонированного репозитория запустить команду

```sh
docker build -t <имя образа> .
```

### Из репозитория Docker Hub
```sh
docker push acglearn/cli:latest 
```

## Конфигурирование

Приложение конфигурируется с помощью одного конфиг-файла вида:
```txt
word_separator ":"
default_range "1-3"
```

* **word_separator**: используемый разделитель слов (по умолчанию "пробел")
  the project size calculation.
* **default_range**: диапазон строк по умолчанию

Путь по умолчанию: **cli.conf**


## Использование

Запустить приложение 

```sh
docker run 
```
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
