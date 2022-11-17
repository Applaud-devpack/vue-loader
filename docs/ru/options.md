---
sidebar: auto
---

# Настройки

## transformAssetUrls

- Тип: `{ [tag: string]: string | Array<string> }`
- По умолчанию:

  ``` js
  {
    video: ['src', 'poster'],
    source: 'src',
    img: 'src',
    image: ['xlink:href', 'href'],
    use: ['xlink:href', 'href']
  }
  ```

  Во время компиляции шаблона, компилятор может преобразовывать определённые атрибуты, такие как URL-адреса в `src`, в вызовы `require`, чтобы указанный ресурс обрабатывался с помощью webpack. Например, `<img src="./foo.png">` будет пытаться найти файл `./foo.png` в вашей файловой системе и добавит его как зависимость сборки.

## compiler

- Тип: `VueTemplateCompiler`
- По умолчанию: `require('vue-template-compiler')`

  Переопределение компилятора по умолчанию, используемого для компиляции секций `<template>` в однофайловых компонентах.

## compilerOptions

- Тип: `Object`
- По умолчанию: `{}`

  Настройки для компилятора шаблонов. При использовании по умолчанию `vue-template-compiler` вы можете использовать эту опцию чтобы добавить пользовательские директивы компилятора, модули, или с помощью `{ preserveWhitespace: false }` игнорировать пробелы между тегами.

  Подробнее в [перечне опций `vue-template-compiler`](https://github.com/vuejs/vue/tree/dev/packages/vue-template-compiler#options).

## transpileOptions

- Тип: `Object`
- По умолчанию: `{}`

  Опции транспиляции ES2015+ в ES5 для генерируемого кода render-функций. [Транспилятор](https://github.com/vuejs/vue-template-es2015-compiler) — форк [Buble](https://github.com/Rich-Harris/buble), поэтому полный список опций вы можете посмотреть [здесь](https://buble.surge.sh/guide/#using-the-javascript-api).

  Компиляция render-функций шаблонов поддерживает специальную трансформацию `stripWith` (включена по умолчанию), которая удаляет использование `with` в сгенерированных render-функциях, чтобы сделать их совместимыми со strict-режимом.

## optimizeSSR

- Тип: `boolean`
- По умолчанию: `true` когда конфигурация webpack имеет свойство `target: 'node'` и версия `vue-template-compiler` 2.4.0 или выше.

  Включает оптимизацию для серверного рендеринга в Vue 2.4, которая компилирует в простые строки часть деревьев vdom, возвращаемых render-функциями, что улучшает производительность серверного рендеринга. В некоторых случаях вам может потребоваться явно отключить оптимизацию, поскольку результирующие render-функции могут быть использованы только для серверного рендеринга и не могут использоваться для рендеринга на стороне клиента или тестирования.

## hotReload

- Тип: `boolean`
- По умолчанию: `true` в режиме разработки, `false` в режиме production или при установленной опции `target: 'node'` в конфигурации webpack.
- Разрешённые значения: `false` (`true` не заставит работать горячую перезагрузку ни в режиме production, ни когда `target: 'node'`)

  Возможность webpack по [горячей перезагрузке модулей](https://webpack.js.org/concepts/hot-module-replacement/) позволяет применять изменения в браузере **без необходимости обновления страницы**.
  Используйте эту опцию (со значением `false`) чтобы отключить горячую перезагрузку в режиме разработки.

## productionMode

- Тип: `boolean`
- По умолчанию: `process.env.NODE_ENV === 'production'`

Позволяет принудительно включить режим production, который исключит из сборки код, необходимый только на этапе разработки (например, горячую перезагрузку модулей).

## shadowMode

- Тип: `boolean`
- По умолчанию: `false`

Компилирует компонент для использования внутри Shadow DOM. В этом режиме стили компонента внедряются в `this.$root.$options.shadowRoot` вместо head-тега документа.

## cacheDirectory / cacheIdentifier

- Тип: `string`
- По умолчанию: `undefined`

Когда указаны оба параметра, включает кэширование в файловой системе скомпилированных шаблонов (требуется, чтобы `cache-loader` был установлен в том же проекте).

::: tip СОВЕТ
  Взаимодействие между `vue-loader` и `cache-loader` использует [инлайновый синтаксис импорта загрузчиков](https://webpack.js.org/concepts/loaders/#inline) внутри, где символ `!` будет рассматриваться как разделитель между различными загрузчиками. Поэтому убедитесь, что `cacheDirectory` не содержит символов `!`.
:::

## prettify

- Тип: `boolean`
- По умолчанию: `true`

В режиме разработки по умолчанию используется [prettier](https://prettier.io/) для форматирования скомпилированной render-функции для удобства отладки. Однако, если вы столкнётесь с какой-либо непонятной ошибкой самого prettier, такой как [экспоненциальное время компиляции для глубоко вложенных функций](https://github.com/prettier/prettier/issues/4672), можно отключить эту опцию.

## exposeFilename

- Тип: `boolean`
- По умолчанию: `false`

В не-production окружениях vue-loader внедряет свойство `__file` в компоненты для улучшения отладки. Если свойство `name` отсутствует в компоненте, Vue будет использовать значение из `__file` для отображения предупреждений в консоли.

Это свойство удаляется из сборки для production по умолчанию. Но вы можете сохранить его, если вы разрабатываете библиотеку компонентов и не хотите указывать `name` в каждом компоненте. В таком случае вы можете включить эту опцию.