![haojiyou](http://hbimg.b0.upaiyun.com/c0b233344592936874714e5596736eb92ae8d3309ff4-QFVcuc_fw658)

# PLATO

> :heart: a Boilerplate for SPAs use vue, [vuex-fsa](https://www.npmjs.com/package/vuex-fsa), vue-router

[See online demo](http://crossjs.com/plato)

:construction: coding hard, coding with :heart:

- :white_check_mark: Plugins like vuex, i18n, validator and ajax are almost ready, could be used in production amusedly.
- :negative_squared_cross_mark: UI components are NOT stable, could be changed anytime.

[![Travis](https://img.shields.io/travis/crossjs/plato.svg?style=flat-square)](https://travis-ci.org/crossjs/plato)
[![Coveralls](https://img.shields.io/coveralls/crossjs/plato.svg?style=flat-square)](https://coveralls.io/github/crossjs/plato)
[![dependencies](https://david-dm.org/crossjs/plato.svg?style=flat-square)](https://david-dm.org/crossjs/plato)
[![devDependency Status](https://david-dm.org/crossjs/plato/dev-status.svg?style=flat-square)](https://david-dm.org/crossjs/plato#info=devDependencies)

## Change Log

- 20160619
  - 使用 icomoon 管理图标字体，因为 iconfont 不支持连体字符

## Dependencies

- vue
  - vuex-fsa
  - vuex-promise
  - vue-router
  - plugins/i18n
  - plugins/validator
  - plugins/ajax
- postCSS
- webpack
- karma
- mocha

## Principles

- 使用 ES6 编写
- 使用 .vue 单文件组件
  - 逻辑尽量写在 script 里，保持 template 逻辑简单
- 向 vue@2 靠拢
- **不限制使用何种 UI 组件，可以使用第三方，或自己开发（请尽量考虑复用性）**
- 尽量使用小的依赖库

## Data

- 使用 [vuex](https://github.com/vuejs/vuex/) 进行数据管理
- 数据按模块分文件存放于 `src/vx/modules` 目录，并确保模块间数据不互相干扰
- 严格遵循单向数据流
- 组件间通过事件传递数据
  - 使用 $emit
  - 禁止使用 $dispatch 或 $broadcast，因为此特性在 vue@2 中不可用

``` js
// file: src/vx/store.js
import Vue from 'vue'
import Vuex from 'vuex-fsa'
import modules from './modules'
import middlewares from './middlewares'

Vue.use(Vuex)

export default new Vuex.Store({
  strict: process.env.NODE_ENV === 'development',
  modules,
  middlewares
})
```

## Router

- 使用 [vue-router](https://github.com/vuejs/vue-router/) 进行路由管理
- 路由按模块分文件存放于 `src/routes` 目录
- 配合 Webpack 实现 [动态载入](http://router.vuejs.org/zh-cn/lazy.html)

## Localization

``` js
// use plugin
import Vue from 'vue'
import I18n from 'plugins/i18n'
Vue.use(I18n)

// set resources
export default {
  name: 'App',
  i18n: {
    resources: {
      message: {
        plato: '...'
      }
    }
  }
}
```

``` vuex
// use translate
// see: https://github.com/Matt-Esch/string-template
<template>
{{__('message.plato')}}
</template>
```

## Validation

``` js
// use plugin
// details in `src/index.js`
import Vue from 'vue'
import Validator from 'plugins/validator'

Vue.use(Validator)

// set validator
// details in `src/views/login.vue`
export default {
  ...
  validator: {
    // 设置初始化后自动检查
    auto: true
  }
  ...
}

// use validator
// in `src/components/c-textfield.vue`
// template:
<input ...
  @change="_validate">

// script:
methods: {
  _validate () {
    if (!this.validate || !this.$validation) {
      return
    }
    this.$validate()
  }
}
```

## Ajax

Based on fetch

``` js
// use plugin
// details in `src/index.js`
import Vue from 'vue'
import Ajax from 'plugins/ajax'

Vue.use(Ajax)

// set ajax,
// details in `src/views/app.vue`
export default {
  // optional
  ajax: {
    ...
  }

  ready () {
    this.$ajax(...)
    this.$GET(...)
    this.$POST(...)
    this.$DELETE(...)
    this.$PATCH(...)
    this.$PUT(...)
  }
}
```

## Theming

- use [postcss](http://postcss.org/), for the future
- use [scoped css](http://vue-loader.vuejs.org/en/features/scoped-css.html) carefully
  - do NOT use `@import` in scoped css
- *modifying `postcss-cssnext` to adjust compatibilities：*

``` js
// .tools/webpack/index.js
require('postcss-cssnext')({
  browsers: 'last 1 version'
}),
```

## Usage

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:3000
npm run dev

# serve with mocking. see mocks in /apis
npm run dev:mock

# clean
npm run clean

# build for production with minification
npm run compile

# run server in production
npm start

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests, currently only run unit
npm test
```

## Compatibility

Latest mobile browsers

## Appendix

- [vue-devtools](https://github.com/vuejs/vue-devtools)
