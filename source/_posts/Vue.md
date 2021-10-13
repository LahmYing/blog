---
title: Vue
date: 2021-08-26 19:40:09
tags: [vue]
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

js + type

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

Compiler: template string -> AST -> code

只要有 template string，包括 SFC，都需要 Compiler

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

## 生产中是有 new Vue() 的

在 dist 中搜 new Vue() 即知, vue.runtime.min.js 被嵌在 app.[hash].js 里

## Vue 定义

src/core/instance/index.js

## 数据驱动的一些重要流程

`new Vue(el, ...) -> vm.$option.el -> vm.$mount` 初始化生命周期、事件中心、渲染、data、props、computed、watcher 等等

`template in el -> render() -> vm.$mount` 无论 SFC 的 还是 js 中的 template，都会转为 render()，然后再 $mount。 `el 挂载点`

### $mount

`Watcher(vm._render 生成虚拟 Node -> vm._update 更新 DOM) -> mounted hook` 初始化和监控的数据变动时触发 Watcher

### `vm._render`

`vm._render -> render(内调用 createElement -> return vnode -> new VNode)`

### VNode 定义

src/core/vdom/vnode.js

VNode and children VNode -> VNode tree

### `vm._update`

`vm._update -> vm.__patch__` 最后调用 DOM api: insertBefore、appendChild 等，插入顺序先子后父

## 组件化的一些重要流程

### createComponent

createComponent 和 createElement 的目的都是产出 VNode
需要注意的是和普通元素节点的 vnode 不同，组件的 vnode 是没有 children vnode 的

内逻辑

- 创建一个 Vue 子类 Sub
- merge options
- install hooks
- return component VNode
- new VNode

### 生命周期

#### beforeMount mounted

`beforeMount hook -> 产出 VNode -> patch -> mounted hook`

最后的 mounted hook，如果是组件 VNode 走到这一步，各个组件的 mounted hook 会被 push 到一个 queue，排队执行，先子后父

#### beforeUpdate & updated

mounted -> beforeUpdate hook -> watcher 执行完我们定义的程序 -> updated hook

#### destroy

destroy 钩子函数执行顺序是先子后父，和 mounted 过程一样

### 全局/局部组件

Vue.options.components
vm.$options.components

### 异步组件

估计：编译分 chunk 时标记 require、()=>import 的组件在哪个 chunk，到时就到哪个 chunk 找
加载异步组件 -> vm.$forceUpdate()

#### patch

会先给异步组件占位，方便后续的 patch 和 $forceUpdate()

## 响应式的一些重要流程

patch 是一个异步过程， VNode -> nextTick -> DOM

修改 msg 后 template 中 `<div id="MSG"> {{ msg }} </div>` 也正确修改了，此时浏览器上显示的 msg 也是修改后的，但是此时 `这个 tag 的 DOM 还没更新！`，console 下即知

```js
this.$refs.MSG; // DOM 未更新
this.nextTick(() => {
  this.$refs.MSG; // DOM 已更新，依赖该 DOM 的代码需在此处执行
});
this.$refs.MSG; // DOM 未更新
```

## 扩展

### event

只有组件节点才可以添加自定义事件，并且添加原生 DOM 事件需要使用 native 修饰符；而普通元素使用 .native 修饰符是没有作用的，也只能添加原生 DOM 事件

### slot

普通插槽和作用域插槽作用域的不同源于 `VNode` 在哪里渲染

VNode 若直接在父组件渲染，则数据挂在父组件的 $options 下，子组件肯定不能直接访问

作用域插槽是把部分数据相关的 VNode 也放到子组件中进行渲染，所以子组件就能直接访问

### keep-alive

- 使用 slot
- 缓存了 VNode
- patch 时缓存过的组件不走 mounted hook

## Vue-Router

Vue-Router 的 install 方法（使用 Vue.mixin，mixin 作用是合并 options）会给每一个组件注入 beforeCreate 和 destoryed 钩子函数，在 beforeCreate 做一些私有属性定义和路由初始化工作

# 其他

## 覆盖组件库 ivew 样式

main.js 中。 **_跟 CSS 文件的引用次序有关_**

```js
import "view-design/dist/styles/iview.css";
// 定制主题色
import "./assets/customTheme/index.less";
```

## vue 插件

https://github.com/vuejs/awesome-vue

## 样式覆盖、Vue 的 scoped 和 /deep/

{% asset_img Vue的scoped和deep.png 1000 1000 image desc %}

## 特性

{% asset_img VueFeature.png 1000 1000 %}

## Vue 中异步传 props 丢失数据

- 子组件加上 v-if, 如

```html
<Child v-if="task.taskType === 21" :taskDetail="task" />
```

- 子组件中使用 watch

## 引入 vue.min.js 时 vue-devtools 会失效

## Vue router history mode

需要服务端支持，只需要服务端在遇到任何路由都返回 index.html 即可（前端为单页应用的话）

## vuex

默认情况下，模块内部的 action、mutation 和 getter 是注册在全局命名空间的——这样使得多个模块能够对同一 mutation 或 action 作出响应

## nextTick

值更新， 值对应的 dom 未更新， 此时你想基于`更新后的 dom`执行 A 函数，需将 A 函数放于 nextTick 内

```vue
<template>
  <div class="channel-manage">
    channel-manage
    <div id="next-tick-html">{{ "showDelModal: " }}{{ showDelModal }}</div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Watch } from "vue-property-decorator";
import { State, Getter, Action } from "vuex-class";

// required even empty
@Component({})
export default class ChannelList extends Vue {
  showDelModal = false;

  // life hook
  created() {
    this.showDelModal = true;

    console.log("no-next-tick-value", this.showDelModal);
    console.log("no-next-tick-dom", document.getElementById("next-tick-html"));
    this.$nextTick(() => {
      console.log("next-tick-value", this.showDelModal);
      console.log("next-tick-dom", document.getElementById("next-tick-html"));
    });
  }
}
</script>
```
