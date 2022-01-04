---
title: css 杂
date: 2021-08-26 19:29:26
tags: [css]
category: [css]
---

<!-- toc -->

# 0.5px

移动端 h5 上，1px 的 border 太粗（1px 可能等于 2、3 物理像素甚至更多）

```css
/* 直接加 class 即可用 */
[class*="mri-hairline"] {
  position: relative;
}

[class*="mri-hairline"]::after {
  position: absolute;
  box-sizing: border-box;
  content: " ";
  pointer-events: none;
  top: -50%;
  right: -50%;
  bottom: -50%;
  left: -50%;
  border: 0 solid #ddd;
  -webkit-transform: scale(0.5);
  transform: scale(0.5);
}

.mri-hairline--top::after {
  border-top-width: 1px;
}

.mri-hairline--right::after {
  border-right-width: 1px;
}
```

# css3 动画

## animation

```css
div {
  animation-name: myfirst;
  animation-duration: 5s;
  /* 速度曲线 */
  animation-timing-function: linear;
  animation-delay: 2s;
  /* 播放的次数 */
  animation-iteration-count: infinite;
  /* 是否在下一周期逆向地播放 */
  animation-direction: alternate;
  /* 是否正在运行或暂停 */
  animation-play-state: running;
}
```

# js css 共享变量

[Javascript 如何与 Sass,Less,Css 之间共享变量？](https://segmentfault.com/a/1190000018795983?utm_source=tag-newest)
[sass-resources-loader](https://www.npmjs.com/package/sass-resources-loader)

# css 逻辑属性

## -inline 和 -block

`{margin/padding/border}-{inline/block}`

- `-inline` -left 加 -right，跟书写方向有关，`margin-inline: 5px 10px`, 从左到有 margin-left 为 5px，右到左为 10px

- `-block` -top 加 -bottom，其余同上，eg ` border-block: 8px solid blue;`

## inline-size 和 block-size

```js
width	inline-size
max-width	max-inline-size
min-width	min-inline-size
height	block-size
max-height	max-block-size
min-height	min-block-size
```

## Position

```js
top	inset-block-start
bottom	inset-block-end
left	inset-inline-start
right	inset-inline-end
top and bottom	inset-block
left and right	inset-inline
```

# 书写方向

css

`writing-mode: horizontal-tb | vertical-rl | vertical-lr | sideways-rl | sideways-lr`

HTML

尽量使用 HTML 属性以防 css 无法加载

`<html dir="rtl">`

# css 风格

## SMAcss

[SMAcss](https://link.segmentfault.com/?url=https%3A%2F%2Fsmacss.com%2F)

- Base
- Layout
- Module
- State
- Theme

## css module

以 webpack 为例，使用 css-loader 就可以实现 css module

## styled-components

样式组件，all-in-js 的理念，不好阅读

```js
import styled from "styled-components";
import styles from "./style.less";

const Wrapper = styled(div)`
  border: 1px dashed ${(props) => props.color};
  width: 100%;
`;

const Header = (props) => {
  return (
    <div>
      {/* 直接看 jsx，看不出来 Wrapper 的原始标签是 div */}
      <Wrapper color="#000">使用 styled-component </Wrapper>
      <div className={styles.Wrapper}>使用 css Modules</div>
    </div>
  );
};
```

# 设备分辨率

[PC 端常见分辨率](https://blog.csdn.net/weixin_30340745/article/details/97680702?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link)

```
1024*600            （常见8、9寸电脑使用)
1024*768            （常见10.4、12.1、14.1、15寸电脑使用）
1280*1024            (常见14.1、15寸电脑使用）
1280*800            （常见10.8、12.1、15.4寸电脑使用）
1280*854            （不常见)
1366*768            （常见15.2寸电脑使用)（主流）
1440*1050            (常见15、16.1寸电脑使用)
1440*900            （苹果17寸电脑)
1600 * 900            (非主流)
1600*1200            (常见15、16.1寸电脑使用)
1600*1024            (不常见)
1680*1050            (常见15.4、20寸电脑)
1920*1080            (主流)
1920*1200            (常见20寸电脑)
```

# 基础

```javascript
/*********************** 背景图片示例 ***********************/
background:lightgray url(doge.jpg) repeat-x;


/*********************** css 的使用 ***********************/
// 内联
<h1 style='background:red;'>内联</h1>

// <head> 标签内的 <style> 标签
<head>
    <meta charset="utf-8">
    <title>fe 16</title>
    <style>
        .c {
            transform: translate(20px, 40px);
        }
    </style>
</head>

// <link> 标签中的外联
<link rel="stylesheet" href="fe6.css">


/*********************** 三种主要的选择器 ***********************/
<span class="c-class" id='c-id'>c</span>
// 元素选择器
span {
}

// class 选择器
.c-class {
}

// id 选择器
#c-id {
}


/*********************** 优先级 ***********************/
// 样式优先级(从高到低)
    !important
    内联样式
    <style> 中的样式
    link 中的样式

// 选择器优先级(从高到低)
    !important
    内联样式
    id 选择器
    class 选择器
    元素选择器


/*********************** display 属性 ***********************/
// block
block 占一行

// inline
inline 只占 content 的尺寸

// inline-block
最常用
inline-block 对外表现为 inline，所以可以和别的 inline 放在一行
对内表现为 block，所以可以设置自身的宽高


/*********************** 盒模型 ***********************/
// inline 元素没有盒模型


/*********************** 定位 ***********************/
// position 属性用于元素定位
    static
    relative
    absolute
    fixed

非 static 元素可以用 top left bottom right 属性来设置坐标
非 static 元素可以用 z-index 属性来设置显示层次

// relative
相对定位

// absolute
完全绝对定位, 忽略其他所有东西
往上浮动到 非 static 的元素

// fixed
基于 window 的绝对定位, 不随页面滚动改变


/*********************** overflow 属性 ***********************/
// visible 默认

// auto
需要的时候加滚动条

// hidden
隐藏多余元素

// scroll
就算用不着也会强制加滚动条


/*********************** 盒模型相关的 css ***********************/
// 盒模型相关的 css

// 建议
// 1.简写
// 2.html 上操作再 copy
// 3.或 css 生成网站上生成

// 简写示例
border: 3px red solid;
background: #233 url(bg.png) no-repeat;


/*********************** 居中写法 ***********************/
// block 元素居中
margin: 0 auto;

// inline inline-block 元素居中
text-align: center;


/*********************** 下划线 ***********************/
text-decoration:
    underline
    overline
    line-through
    blink(这个值已经废弃了)


/*********************** margin 合并 ***********************/

```

# css3 动画

```html
<!DOCTYPE html>
<html>
  <!--
css3 动画的套路(主要是定了一套测试动画的方案)
    translate 优先于 rorate
    animationend 事件
        在动画播完后触发
        动画播放被暂停不会触发
    animationiteration 事件
        在动画播放一轮后触发
        如果动画只播放一轮, 那么不会触发此事件
    利用事件测试动画
-->

  <head>
    <meta charset="utf-8" />
    <title>css3测试动画方案</title>
    <style>
      .gua-block {
        background: lightblue;
        width: 100px;
        height: 100px;
      }

      .gua-spin {
        animation: spin linear 2s;
        animation-iteration-count: 1;
      }

      @keyframes spin {
        0% {
          transform: rotate(0deg);
        }

        100% {
          transform: rotate(160deg) translate(0, 50px);
        }
      }
    </style>
  </head>

  <body>
    <div class="gua-block">方块</div>
    <button class="play">播放动画</button>
    <script>
      var e = function (sel) {
        return document.querySelector(sel);
      };

      var playAnimation = function () {
        var animation = "gua-spin";
        var block = e(".gua-block");
        // 让它开始播放动画
        block.classList.add(animation);
        // 绑定一个 animationend 事件, 在动画结束后删除动画 class
        block.addEventListener("animationend", function () {
          block.classList.remove(animation);
        });
      };

      var __main = function () {
        e(".play").addEventListener("click", function (e) {
          playAnimation();
        });
      };

      __main();
    </script>
  </body>
</html>
```

# css 生成器

```javascript
/*************************************
keyframes 动画和生成器
 ************************************/
https://daneden.github.io/animate.css/
http://cssanimate.com/



/*************************************
其他 css3 生成器
 ************************************/
http://css3generator.com/
http://www.css3generator.in/
http://css3.me/
https://www.tutorialspoint.com/css/css3_boarder_image.htm
https://www.html.cn/tool/css3Preview/

```

# 优先级

- !important > 内联 > ID 选择器 > 类选择器 > 标签选择器
  - 尽量不要在内联样式中使用 !important

### 计算方式

- a, b, c, d
  如果有内联样式，a = 1 ，否则 a = 0，b 为 ID 选择器出现的次数，c 为类选择器、属性选择器和伪类出现的总次数，d 为标签选择器和伪元素出现的总次数
- 规则
  从左到右进行比较，数值较大的优先；如果相等，则向右比较下一位，数值较大的优先；如果 4 位数全部相等，则后声明的优先

# 选择器

[http://www.ruanyifeng.com/blog/2009/03/css_selectors.html](http://www.ruanyifeng.com/blog/2009/03/css_selectors.html)

## 通配选择器

星号 `*`， 用于匹配 html 文档内的所有元素，在搭配其他选择器使用时，会被完全忽略掉

## 属性选择器

选择器[属性条件] { 声明 }

### 属性条件

class 也可以这样操作，如 [class* = 'col-']

- [attr] 匹配属性名为 attr 的元素
- [attr=value]
- [attr~=value] 属性值里至少有一个 = value，比如 `<div attr="0 1 2">` 有 3 个值
- [attr|=value] 属性值为“value”或是以“value-”为前缀
- [attr^=value] 属性值是以"value"开头
- [attr$=value] 属性值是以"value"结尾
- [attr*=value] 属性值包含有"value"，不严格匹配，如 属性值 = iosAndroid，value 可为 osA、iosA 等

## 后代

`div p {}` 匹配 div 下的 所有 p

## 子

`元素 1 > 元素 2 { 声明 }` 子选择器只会匹配到下一级的元素而后代选择器是匹配到所有的后代元素不管 dom 的层级有多深

## 通用兄弟

位置无须紧邻，只须同层级，`A~B` 选择 A 元素之后【所有】同层级 B 元素

## 相邻兄弟

`A + B` 当 B 紧跟在 A 之后，并且 A B 都是属于同一个父元素的子元素，则 B 将被选中

## 选择器使用伪类、伪元素

一个选择器可以同时使用多个伪类，但只能同时使用一个伪元素

- 常见
  - :hover 鼠标悬停
  - :focus 焦点
  - :first-child 表示在一组兄弟元素中的第一个元素
  - :last-child 表示在一组兄弟元素中的最后一个元素
  - :nth-child(an + b) 第 an+b 个

# margin 合并

margin 合并是取 margin 较大一方的值，而不是相加

## 合并前提

[Mastering margin collapsing](https://developer.mozilla.org/en-US/docs/Web/css/css_Box_Model/Mastering_margin_collapsing)

- 同一 BFC 内相邻的 Block-Level 的元素
  - floating 和 absolutely positioned 的元素不会

# 清除 float

[如何清除浮动](https://github.com/ljianshu/Blog/issues/16)

- 使 float 的父 div 成为 BFC
- 伪类/伪元素 + clear

```html
<div id="wrap" class="clearfix">
  <div id="inner"></div>
</div>

<style>
  #wrap {
    border: 1px solid;
  }

  #inner {
    float: left;
    width: 200px;
    height: 200px;
    background: pink;
  }

  .clearfix::after {
    content: "";
    display: block;
    clear: both;
  }
</style>
```
