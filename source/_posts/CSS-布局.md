---
title: 三栏布局
date: 2021-11-11 17:28:53
tags: [css]
category: [css]
---

<!-- toc -->

推荐程度由高至低

## flex 布局

- 支持 flex 的优先 flex，支持 **IE10** 及以上

## 表格布局

- 兼容性很好
- 缺点
  - 无法设置 **栏边距**
  - 对 seo 不友好
  - 当其中一个单元格高度超出的时候，两侧的单元格也是会跟着一起变高的

```html
<!--表格布局-->
<section class="layout table">
  <style>
    .layout.table .left-center-right {
      display: table;
      height: 150px;
      width: 100%;
    }

    .layout.table .left-center-right > div {
      display: table-cell;
    }

    .layout.table .left {
      width: 300px;
      /* 其他两个 table-cell 的高度也被撑开到 400px */
      height: 400px;
      background: red;
    }

    .layout.table .center {
      background: yellow;
    }

    .layout.table .right {
      width: 300px;
      height: 100px;
      background: blue;
    }
  </style>
  <h1>三栏布局</h1>
  <article class="left-center-right">
    <div class="left"></div>
    <div class="center">
      <h2>表格布局解决方案</h2>
      1.这是三栏布局的浮动解决方案； 2.这是三栏布局的浮动解决方案；
      3.这是三栏布局的浮动解决方案； 4.这是三栏布局的浮动解决方案；
      5.这是三栏布局的浮动解决方案； 6.这是三栏布局的浮动解决方案；
    </div>
    <div class="right"></div>
  </article>
</section>
```

## 浮动布局

- 先写左右浮动 div，再写中间块的 div，避免右浮动被挤到下一行。浮动 div 要记得 **清除浮动** ，避免 **容器高度塌陷**

## 绝对布局

- 三个 div 都 absolute，后代 div **在高度未知时容易出问题** ，该布局可用性较差

## 网格布局

- 最强大，但兼容性不好。IE10+上支持，而且也仅支持部分属性

## 参考

[https://github.com/ljianshu/Blog/issues/14](https://github.com/ljianshu/Blog/issues/14)

[几种常见的 CSS 布局](https://github.com/ljianshu/Blog/issues/40)
