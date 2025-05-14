---
title: Vue2.x 源码解读
date: 2021-08-26 19:40:09
tags: [vue]
category: [vue]
---

<!-- toc -->

# Vue2.x 源码解读

https://ustbhuangyi.github.io/vue-analysis/v2/prepare/

## package.json

https://github.com/vuejs/vue/blob/dev/package.json

```json
"devDependencies": {
    "@babel/core": "^7.0.0",
    "@babel/plugin-proposal-class-properties": "^7.1.0",
    "@babel/plugin-syntax-dynamic-import": "^7.0.0",
    "@babel/plugin-syntax-jsx": "^7.0.0",
    "@babel/plugin-transform-flow-strip-types": "^7.0.0",
    "@babel/preset-env": "^7.0.0",
    "@babel/register": "^7.0.0",
    "@types/node": "^12.12.0", // 声明文件
    "@types/webpack": "^4.4.22", // 声明文件
    "acorn": "^5.2.1", // js -> AST
    "babel-eslint": "^10.0.1",
    "babel-helper-vue-jsx-merge-props": "^2.0.3",
    "babel-loader": "^8.0.4",
    "babel-plugin-istanbul": "^5.1.0",
    "babel-plugin-transform-vue-jsx": "^4.0.1",
    "babel-preset-flow-vue": "^1.0.0",
    "buble": "^0.19.3", // js -> es5, 基本都支持 es6 除了 IE11
    "chalk": "^2.3.0",
    "chromedriver": "^2.45.0", // for e2e test
    "codecov": "^3.0.0", // 测试结果可视化
    "commitizen": "^2.9.6", // 规范 commit message, 提交工具
    "conventional-changelog": "^1.1.3", // 规范 commit message, 约定格式的适配器
    "cross-spawn": "^6.0.5", // node 中，child_process.spawn 使用指定的命令行参数创建新进程，"cross-spawn": A cross platform solution to node's spawn and spawnSync
    "cz-conventional-changelog": "^2.0.0", // 规范 commit message, 约定格式的适配器
    "de-indent": "^1.0.2", // 从代码块中删除额外的缩进
    "es6-promise": "^4.1.0", // This is a polyfill of the ES6 Promise
    "escodegen": "^1.8.1", // AST -> js
    "eslint": "^5.7.0",
    "eslint-plugin-flowtype": "^2.34.0",
    "eslint-plugin-jasmine": "^2.8.4",
    "file-loader": "^3.0.1",
    "flow-bin": "^0.61.0",
    "hash-sum": "^1.0.2",
    "he": "^1.1.1", // HTML encoder/decoder
    "http-server": "^0.12.3",
    "jasmine": "^2.99.0", // 单元测试工具
    "jasmine-core": "^2.99.0",
    // 一个可以在多个浏览器中执行js代码的简单工具。它不是一个完整的测试框架，没有断言库(比如jasmine或者mocha)，只是启动了一个http服务器，然后生成测试html文件，执行测试用例的js
    "karma": "^3.1.1",
    "karma-chrome-launcher": "^2.1.1",
    "karma-coverage": "^1.1.1",
    "karma-firefox-launcher": "^1.0.1",
    "karma-jasmine": "^1.1.0",
    "karma-mocha-reporter": "^2.2.3",
    "karma-phantomjs-launcher": "^1.0.4",
    "karma-safari-launcher": "^1.0.0",
    "karma-sauce-launcher": "^2.0.2",
    "karma-sourcemap-loader": "^0.3.7",
    "karma-webpack": "^4.0.0-rc.2",
    "lint-staged": "^8.0.0",
    "lodash": "^4.17.4",
    "lodash.template": "^4.4.0",
    "lodash.uniq": "^4.5.0",
    "lru-cache": "^5.1.1", // A cache object that deletes the least-recently-used items
    "nightwatch": "^0.9.16",  // web-ui自动化测试框架, for e2e test
    "nightwatch-helpers": "^1.2.0",
    "phantomjs-prebuilt": "^2.1.14", // Headless WebKit with JS API, 建议换成 Headless Chrome
    "puppeteer": "^1.11.0", // 控制 headless Chrome
    "resolve": "^1.3.3",
    "rollup": "^1.0.0",
    "rollup-plugin-alias": "^1.3.1",
    "rollup-plugin-buble": "^0.19.6",
    "rollup-plugin-commonjs": "^9.2.0",
    "rollup-plugin-flow-no-whitespace": "^1.0.0",
    "rollup-plugin-node-resolve": "^4.0.0",
    "rollup-plugin-replace": "^2.0.0",
    "selenium-server": "^2.53.1", // exports the selenium server path
    "serialize-javascript": "^3.1.0", // Serialize-javascript 能够序列化 JavaScript 库中含有正则表达式和功能的 JSON 包
    "shelljs": "^0.8.1", // 在javascript代码中编写shell命令实现功能
    "terser": "^3.10.2", // A JavaScript parser and mangler/compressor toolkit for ES6+
    "typescript": "^3.6.4",
    "webpack": "~4.28.4",
    "weex-js-runtime": "^0.23.6",
    "weex-styler": "^0.3.0",
    "yorkie": "^2.0.0" // yorkie实际是fork husky，然后做了一些定制化的改动，使得钩子能从package.json的 "gitHooks"属性中读取
  },
```

## flow

带类型的 js

```js
/*@flow*/
```

## 目录

```
src
├── compiler # 编译相关
├── core # 核心代码
├── platforms # 不同平台的支持
├── server # 服务端渲染
├── sfc # .vue 文件解析 -> js obj
├── shared # 共享代码
```

## Runtime Only VS Runtime + Compiler

Compiler: template string -> AST -> code，只要有 template string，包括 SFC，都需要 Compiler

Runtime 在运行时处理 `new Vue()` 等代码，构建后 `vue.runtime.min.js` 一般被嵌在 `app.[hash].js` 里

```js
// 需要编译器的版本
new Vue({
  template: "<div>{{ hi }}</div>",
});

// 这种情况不需要
new Vue({
  render(h) {
    return h("div", this.hi);
  },
});
```

## 数据驱动（从 Vue 实例化到 DOM 更新）

### 总览

{% asset_img 数据驱动.jpg %}

### new Vue 到 vm.$mount

{% asset_img 1.new-Vue到vm.$mount.jpg %}

### vm.$mount

{% asset_img 2$mount.png %}

### vm.\_render

{% asset_img 3.vm._render().jpg %}

### vm.\_update

{% asset_img vm._update.png %}

## 组件内部工作流程

### 创建组件类型 VNode

{% asset_img 创建组件类型VNode.jpg %}

### 合并配置

配置最终合并到 vm.$options，发生场景：

- new Vue 的 this.\_init() 即实例化的初始化时
- Vue.extend 即组件化时

### 生命周期

#### 父子组件生命周期执行顺序

- 父 created
- 父 beforeMount
- 子 created
- 子 beforeMount
- 子 mounted
- 父 mounted

#### 钩子执行处

在 `vm.$options` 中执行**钩子数组**，比如 `vm.$options.created`

#### beforeCreate created

beforeCreate -> initState -> created

initState 的作用是初始化 props、data、methods、watch、computed 等属性，所以 beforeCreate 时是访问不到 data props 等的

#### beforeMount mounted

beforeMount hook -> vm.\_render() -> vm.\_update() -> mounted hook

最后的 mounted hook，如果是组件 VNode 走到这一步，各个组件的 mounted hook 会被 push 到一个 queue，排队执行，先子后父

#### beforeUpdate & updated

mounted -> beforeUpdate hook -> 进入 Watcher 实例 -> updated hook

在 Watcher 实例中： vm.\_render() 生成 VNode -> vm.\_update() 更新 DOM

#### beforeDestroy destroy

beforeDestroy -> 一系列销毁动作如删除 watcher、钩子 -> destroy

destroy 钩子函数执行顺序是先子后父，和 mounted 过程一样

### 全局/局部组件

{% asset_img 组件注册.jpg %}

#### 全局组件

- Vue.component
- 检测为组件类型，通过 Vue.extend， 然后赋给 Vue.options.components
- -> Sub.options.components // 子组件是 Vue.extend 而来，Vue.options 被合并到 Sub.options
- 取出 components 并参与 new VNode()

#### 局部组件

- 取出 components 并参与 new VNode()

### 异步组件

webpack 构建 chunk 时标记 require、()=>import('组件') 的组件在哪个 chunk，到时就到哪个 chunk 找

加载异步组件后会通过 forceRender 强制重新渲染

#### patch

会先给异步组件占位，方便后续的 patch 和 $forceUpdate()

## 响应式

### Object.defineProperty

Object.defineProperty(obj, prop, descriptor)

存取型的 descriptor 包含两个 key:

- get: 属性的 getter 函数，访问属性时会触发
- set: 当属性值被修改时，会调用此函数

### 依赖收集 (getter) 派发更新 (setter)

基于 getter setter 和订阅模式来做依赖收集和派发更新，实现数据的响应式

{% asset_img reactive1.jpg %}

{% asset_img reactive.png %}

### nextTick

{% asset_img nextTick.jpg %}

patch 是一个异步过程， VNode -> nextTick -> DOM

修改 msg 后 template 中 `<div id="MSG"> {{ msg }} </div>` 也正确修改了，此时浏览器上显示的 msg 也是修改后的，但是此时**_这个 tag 的 DOM 还没更新_**，console 下即知

```js
this.$refs.MSG; // DOM 未更新
this.nextTick(() => {
  this.$refs.MSG; // DOM 已更新，依赖该 DOM 的代码需在此处执行
});
this.$refs.MSG; // DOM 未更新
```

### 计算属性 侦听属性

- 计算属性其实是 computed watcher

- watcher options
  - immediate: 刷新页面进入组件即触发一次该 watch 回调

## 扩展

### event

只有组件节点才可以添加自定义事件，并且添加原生 DOM 事件需要使用 native 修饰符；而普通元素使用 .native 修饰符是没有作用的，也只能添加原生 DOM 事件

### slot

普通插槽和作用域插槽作用域的不同源于 VNode 在哪里渲染

VNode 若直接在父组件渲染，则数据挂在父组件的 vm.$options 下，子组件肯定不能直接访问

作用域插槽是把部分数据相关的 VNode 也放到子组件中进行渲染，所以子组件就能直接访问

### keep-alive

- 使用 slot
- 缓存了 VNode
- patch 时缓存过的组件不走 mounted hook

## Vue-Router

Vue-Router 的 install 方法（使用 Vue.mixin，mixin 作用是合并 options）会给每一个组件注入 beforeCreate 和 destoryed 钩子函数，在 beforeCreate 做一些私有属性定义和路由初始化工作
