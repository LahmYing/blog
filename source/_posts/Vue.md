---
title: Vue
date: 2021-08-26 19:40:09
tags: [vue]
---

<!-- toc -->

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
