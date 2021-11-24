---
title: css 扩展语言
date: 2021-11-11 17:25:39
tags: [css]
category: [css]
---

<!-- toc -->

## What

CSS 扩展语言，编译为 CSS。有 less、sass、stylus

## Benifit

- 添加编程特性，如 **变量** 、 **函数**
- 减少书写重复的选择器

## Point

- 基本语法
- 嵌套
- 变量
- `@import`
- mixin
- 继承
- 函数
- 逻辑控制

## 嵌套

嵌套都一样

```less
.a {
  &.b {
    color: red;
  }
}
```

編譯後

```css
.a.b {
  color: red;
}
```

引用父級選擇器：&

```
在一个选择器中用两次以上 & 且父选择器是一个列表时，
Less 会对选择器进行排列组合，而 Sass 和 Stylus 不会这么做。

假设上层选择器为 .a, .b，
则内部的 & & 在 Less 中会成为 .a .a, .a .b, .b .a, .b .b，
而 Sass 和 Stylus 则输出 .a .a, .b .b
```

Sass 和 Stylus 分别用 [@at-root ]() 和 / 符号作为嵌套引用时的根选择器

```stylus
.post
  section
    .section-title
      color #333
      /.post .section-link
        color #999
    /* other section styles */

  /* other post styles */
```

編譯後

```css
.post section .section-title {
  color: #333;
}
.post .section-link {
  color: #999;
}
```

## 变量

stylus

```stylus
red = #c00

strong
  color: red
```

Less 的变量名用 @ 开头很可能会和以后的 css 新 @ 规则冲突

## 变量作用域

三种预处理器的变量作用域都是按嵌套的规则集划分，并且在当前规则集下找不到对应变量时会逐级向上查找

变量值

```
less 最后定义的值（这样就可以覆盖引入的第三库的变量，
缺点也很明显，会影响之前的样式，sass、stylus 则不会）；
sass 和 stylus 上一个定义值。
```

## 修改第三方库样式

```sass
// Sass 和 Stylus 都提供了「仅当变量不存在时才赋值」的功能
// 我们要修改的变量
$x: 1;
// 第三方库开发时预留 !default，前提是人家有预留给你
$x: 5 !default;
$y: 3 !default;

// $x = 1, $y = 3
```

## 插值

- 支持变量名插值、选择器插值、[@import ]() 语句插值、属性名插值
- 支持其他 @ 规则插值。三种预处理器均支持在 @media、@keyframes、[@counter-style ]() 等规则中进行插值。[@media ]() 插值主要用来做响应式的配置，而 [@keyframes ]() 这样带名称名称的 @ 规则则可以通过插值来避免命名冲突

## mixin

如果使用 mixin，推荐 sass 和 stylus

sass 的 sass 语法

```sass
// = 表示 mixin
=large-text
  font:
    family: Arial
    size: 20px
    weight: bold
  color: #ff0000

// + 表示引入
.page-title
  +large-text
  padding: 4px
  margin-top: 10px
```

sass 的 scss 语法

```scss
@mixin large-text {
  font: {
    family: Arial;
    size: 20px;
    weight: bold;
  }
  color: #ff0000;
}

// @include 表示引入
.page-title {
  @include large-text;
  padding: 4px;
  margin-top: 10px;
}
```

stylus

```stylus
border-radius(n)
  -webkit-border-radius: n
  -moz-border-radius: n
  border-radius: n

.circle
  border-radius(50%)
```

## 继承

stylus

```stylus
.message
  padding: 10px
  border: 1px solid #eee

.warning
  @extend .message
  color: #e2e21e
```

输出

```stylus
.message,
.warning {
  padding: 10px;
  border: 1px solid #eee;
}
.warning {
  color: #e2e21e;
}
```

继承功能还有一个潜在的问题：继承会影响输出的顺序

sass

```sass
.active {
   color: red;
}
button.primary {
   color: green;
}
button.active {
   @extend .active;
}
```

## 函数

三者调用函数的方式几乎一致，不同之处在于 Sass 和 Stylus 支持直接指定参数名的方式传入参数。以 Stylus 为例：

```stylus
subtract(a, b)
  a - b

subtract(b: 10, a: 25) // same as substract(25, 10)

// 好处：如果参数列表比较长，Stylus 可以直接为列表后面的参数赋值，
// 而不需要一路将之前的参数填上 null 或默认值
```

## 参考

[再谈 CSS 预处理器](https://efe.baidu.com/blog/revisiting-css-preprocessors/)
