---
title: BFC&IFC
date: 2021-11-11 16:55:54
tags: [css]
category: [css]
---

<!-- toc -->

# BFC

## WHAT

- Block formatting context
- 块级格式化上下文
- 产生一块独立的渲染区域，只容纳块级元素。`可理解为一个管理块级元素的容器`

## HOW & WHY

- 如何创建
  - float 为 left | right
  - overflow 为 hidden | auto | scroll （常用）
  - display 为 table-cell | table-caption | inline-block | inline-flex | flex
  - position 为 absolute | fixed
  - 根元素
  - display: flow-root 可创建无副作用的 BFC ，即能避免子代 float 元素溢出的 BFC

```html
<style>
  .box {
    background-color: rgb(224, 206, 247);
    border: 5px solid rebeccapurple;
    display: flow-root;
  }

  .float {
    float: left;
    width: 200px;
    height: 150px;
    background-color: white;
    border: 1px solid black;
    padding: 10px;
  }
</style>

<div class="box">
  <div class="float">I am a floated box!</div>
  <p>I am content inside the container.</p>
</div>
```

- 内部规则
  - 内部块级元素都占一行
  - 元素逐个垂直排序，距离由 margin 决定，margin 会折叠
- 体现的特性
  - BFC 的区域不会与非同一 BFC 的 float box 重叠，即浮动不会影响其它 BFC 中元素的布局
  - 可解决父 div 高度塌陷的问题，将父 div 变成 BFC，float 的子 div 就被套入这个 BFC 了
  - 同一 BFC 内的 float 元素不会与其他 block 元素产生重叠

```html
<style>
  .parent {
    border: solid 5px;
    width: 300px;
    /* 将 parent 变为 BFC */
    /* overflow: hidden; */
  }

  .child:nth-child(1) {
    height: 100px;
    width: 100px;
    background-color: yellow;
    float: left;
  }

  .child:nth-child(2) {
    height: 100px;
    width: 100px;
    background-color: red;
    float: left;
  }

  .child:nth-child(3) {
    height: 100px;
    width: 100px;
    background-color: greenyellow;
    float: left;
  }
</style>
</head>

<div>
  <div class="parent">
    <div class="child"></div>
    <div class="child"></div>
    <div class="child"></div>
  </div>
</div>
```

```html
<style>
  .box1 {
    height: 100px;
    width: 100px;
    float: left;
    background: lightblue;
  }

  .box2 {
    width: 200px;
    background: #eee;
  }
</style>

<body>
  <div class="box1">我是一个左浮动的元素</div>
  <div class="box2">
    喂喂喂!大家不要生气嘛，生气会犯嗔戒的。悟空你也太调皮了，
    我跟你说过叫你不要乱扔东西，你怎么又……你看，我还没说完你就把棍子给扔掉了!
    月光宝盒是宝物，你把它扔掉会污染环境，要是砸到小朋友怎么办，就算砸不到小朋友，
    砸到花花草草也是不对的。
  </div>
</body>
```

# IFC

## WHAT

- Inline Formatting Contexts

## HOW & WHY

- 如何创建 IFC

  - 设置为 inline-block 即可，此时可用 text-align 和 vertical-align 达成水平/垂直居中

# 参考

[https://github.com/ljianshu/Blog/issues/15](https://github.com/ljianshu/Blog/issues/15)

[https://developer.mozilla.org/zh-CN/docs/Web/Guide/css/Block_formatting_context](https://developer.mozilla.org/zh-CN/docs/Web/Guide/css/Block_formatting_context)
