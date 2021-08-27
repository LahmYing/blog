---
title: Vue
date: 2021-08-26 19:40:09
tags: [vue]
---

<!-- toc -->

# 覆盖组件库 ivew 样式

main.js 中。 **_跟 CSS 文件的引用次序有关_**

```js
import "view-design/dist/styles/iview.css";
// 定制主题色
import "./assets/customTheme/index.less";
```

# 样式覆盖、Vue 的 scoped 和 /deep/

{% asset_img Vue的scoped和deep.png 1000 1000 image desc %}

# 特性

{% asset_img VueFeature.png 1000 1000 %}

# Vue 中异步传 props 丢失数据

- 子组件加上 v-if, 如

```html
<Child v-if="task.taskType === 21" :taskDetail="task" />
```

- 子组件中使用 watch

# 引入 vue.min.js 时 vue-devtools 会失效

# Vue router history mode

需要服务端支持，只需要服务端在遇到任何路由都返回 index.html 即可（前端为单页应用的话）
