---
title: vue 杂
date: 2021-11-11 17:46:17
tags: [vue]
category: [vue]
---

<!-- toc -->

## Vue Error in nextTick: "RangeError: Maximum call stack size exceeded"

原因：两个组件的 name 重复了

场景：`export default class Layout extends Vue {}` 引入的组件库已有 Layout 组件

## 覆盖组件库 ivew 样式

main.js 中。 **_跟 CSS 文件的引用顺序有关_**

```js
import "view-design/dist/styles/iview.css";
// 定制主题色
import "./assets/customTheme/index.less";
```

## vue 插件

https://github.com/vuejs/awesome-vue

## 样式覆盖、Vue 的 scoped 和 /deep/

{% asset_img Vue的scoped和deep.png %}

## Vue 中异步传 props 丢失数据

解决方法：

- 子组件加上 v-if, 如 `<Child v-if="task.taskType === 21" :taskDetail="task" />`

- 子组件中使用 watch

## 引入 vue.min.js 时 vue-devtools 会失效

## Vue router history mode

需要服务端支持，只需要服务端在遇到任何路由都返回 index.html 即可（前端为单页应用的话）

## vuex

默认情况下，模块内部的 action、mutation 和 getter 是注册在全局命名空间的——这样使得多个模块能够对同一 mutation 或 action 作出响应

## nextTick

值更新， 值对应的 dom 未更新， 此时你想基于`更新后的 dom`执行 A 函数，需将 A 函数放于 nextTick 内

```html
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
      console.log(
        "no-next-tick-dom",
        document.getElementById("next-tick-html")
      );
      this.$nextTick(() => {
        console.log("next-tick-value", this.showDelModal);
        console.log("next-tick-dom", document.getElementById("next-tick-html"));
      });
    }
  }
</script>
```
