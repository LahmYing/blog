---
title: css
date: 2021-08-26 19:29:26
tags: [css]
---

<!-- toc -->

# js css 共享变量

[https://segmentfault.com/a/1190000018795983?utm_source=tag-newest](https://segmentfault.com/a/1190000018795983?utm_source=tag-newest)
[https://www.npmjs.com/package/sass-resources-loader](https://www.npmjs.com/package/sass-resources-loader)

# css 逻辑属性

## -inline 和 -block

margin/padding/border-inline/block

`-inline` -left 加 -right，跟书写方向有关，`margin-inline: 5px 10px`, 从左到有 margin-left 为 5px，右到左为 10px

`-block` -top 加 bottom，其余同上

eg ` border-block: 8px solid blue;`

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

# PostCss

转译、兼容前缀

# 兼容

```js
/*******
来源
*******/
// https://zhuanlan.zhihu.com/p/24413264


/*******
其他推荐
*******/
// https://juejin.im/entry/5847c21aac502e006b0f8031
// https://segmentfault.com/a/1190000004336869
// https://zhuanlan.zhihu.com/p/25216275
// https://aotu.io/notes/2017/11/27/iphonex/index.html
// https://jixianqianduan.com/frontend-css/2016/01/15/responsive-css.html


/*******
汇总
*******/
// https://www.zhihu.com/question/302297294


/*******
处理兼容问题的思路
*******/
// 要不要做
// -产品的角度（产品的受众、受众的浏览器比例、效果优先还是基本功能优先）
// -成本的角度 (有无必要做某件事)
//
// 做到什么程度
// -让哪些浏览器支持哪些效果
//
// 如何做
// -根据兼容需求选择技术框架/库(如jquery 1.x.x)
// -根据兼容需求选择兼容工具：html5shiv、Respond.js、css Reset、normalize.css、Modernizr.js、 postcss
// -条件注释、css Hack、js 能力检测做一些修补



/*******
渐进增强和优雅降级
*******/


/*******
具体方法
*******/
// IE条件注释
// 注意：只有 IE9以下的浏览器才能识别这种语法，其他浏览器仅仅认为 是普通注释。
// demo
<!--[if IE 6]>
<p>IE6下 这句生效，在其他浏览器下认为是普通注释</p>
<![endif]-->
<!--[if !IE]><!-->
<script>alert("在 IE 下条件语法生效，但script不执行。在非 IE 下上下两句都被当做注释所以当前 script 会执行");</script>
<!--<![endif]-->
<!--[if IE 8]>
<link href="ie8only.css" rel="stylesheet">
<![endif]-->


// css hack
.box{
  color: red;
  _color: blue; /*只有ie6认识*/
  *color: pink; /*只有ie67认识*/
  color: yellow\9;  /*ie浏览器都能识别*/
}


// 常见属性的兼容情况
inline-block: >=ie8
min-width/min-height: >=ie7
:before,:after: >=ie8
div:hover: >=ie7
inline-block: >=ie8
background-size: >=ie9
圆角: >= ie9
阴影: >= ie9
动画/渐变: >= ie10


// 一些兼容写法范例
.clearfix:after{
  content: '';
  display: block;
  clear: both;
}
.clearfix{
  *zoom: 1; /* 仅对ie67有效，zoom:1触发hasLayout,起到类似BFC的效果 */
}


.target{
  display: inline-block;
  *display: inline; /*仅对ie67生效*/
  *zoom: 1; /*仅对ie67生效*/
}


<!--[if lt IE 9]>
    <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
    <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
<![endif]-->


<!DOCTYPE html>
<!--[if lt IE 7 ]> <html class="no-js ie6"> <![endif]-->
<!--[if IE 7 ]>    <html class="no-js ie7"> <![endif]-->
<!--[if IE 8 ]>    <html  class="no-js ie8"> <![endif]-->
<!--[if (gte IE 9)|!(IE)]><!--><html  class="no-js"><!--<![endif]-->


/*******
推荐的 兼容相关的工具/库
*******/
// https://github.com/Modernizr/Modernizr
// https://github.com/postcss/postcss
```

# 响应式

```js
// https://github.com/ljianshu/Blog/issues/38

/*
一、前言
*/
// 适配多种设备和多个屏幕


/*
二、视口
*/
// viewport （视口） ==  浏览器中用于呈现网页的区域。
// 视口通常并不等于屏幕大小，特别是可以缩放浏览器窗口的情况下。
// 电脑端的视口宽度等于分辨率，而移动端的视口宽度跟分辨率没有关系,宽度默认值是设备厂家指定的。
// iOS, Android 基本都将这个视口分辨率设置为 980px。

1.约束视口
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" />
// width=device-width   视口为设备宽度（就是人设置的一个宽度）//不设置的话默认为980px
// initial-scale=1.0    初始化的视口大小是1.0倍
// maximum-scale=1.0    最大的倍数是1.0倍
// user-scalable=0      不允许缩放视口
// 约束后的视口宽度都是在 320~480 之间（手机竖直使用的时候）。
// 这个视口的尺寸，是手机厂商设置的
// 能够保证
//   1)我们的文字比如 16px，在自己的这个视口下清晰、大小刚刚合适。
//   2)我们的网页可以用 px 写字号、写行高。
// 注意：
//   1)约束之后的视口宽度，不是自己的分辨率！！每个手机的分辨率，都要比自己的视口宽度大得多得多！
//   2)前端开发工程师，丝毫不关心手机的分辨率，我们只关心视口




/*
三、图片
*/
我们想让图片能在不同大小的屏幕中自动缩放
img {
 max-width: 100%;
}

为什么用 max-width?
// 保证所有图片最大显示为其自身的 100%（即最大只可以显示为自身那么大）。此时，如果包含图片的元素（比如包含图片的 body 或 div）比图片固有宽度小，图片会缩放占满最大可用空间

为什么不用 width:100%?
// 这条规则会导致它显示得跟它的容器一样宽。在容器比图片宽得多的情况下，图片会被无谓地拉伸。




/*
四、兼容手机浏览器内核
*/
移动端，四个独立的浏览器内核
// 微软的 Trident
// 火狐的 Gecko
// 开源内核 Webkit
// Opera 的 Presto

兼容浏览器内核的前缀：
// 1	-ms-
// 2	-moz-
// 3	-o-
// 4	-webkit-

一般兼容-webkit-即可
// 因为占了绝大部分的市场份额




/*
五、百分比布局也叫作流式布局、弹性盒布局
*/
百分比能够设置的属性是 width、height、padding、margin。
其他属性比如 border、font-size 不能用百分比设置的

如果用百分比写
// width，指的是父元素 width 的百分之多少
// height，那么指的是父元素 height 的百分之多少
// padding，那么指的是父元素 width 的百分之多少，无论是水平的 padding 还是竖直的 padding
// margin，那么指的是父元素 width 的百分之多少，无论是水平的 margin 还是竖直的 margin

用百分比写的 demo
div{
  width:200px;
  height:300px;
  padding:10px;
}
div p{
  width:50%;
  height:50%;
  padding:10%;
}
此时p的真实宽度是多少？
// width = 200px x 50% + 2 x 200px x 10% (左右padding) = 140px
// height = 300px x 50% + 2 x 200px x 10% (上下padding) = 190px
// 此时 p 的真实宽度是 140px*190px




/*
六、媒体查询
*/
// IE6、7、8 不支持媒体查询

1.为什么响应式 Web 设计需要媒体查询
// css3 媒体查询可以让我们
// 针对特定的设备能力或条件
// 为网页应用特定的 css 样式

2.媒体查询语法
// 基本的样式
body {
    background-color: grey;
 }
 // 为不同视口、不同能力的设备，渐进增加不同的视觉效果和功能
@media screen and (min-width:1200px){
    body{
        background-color: pink;
	}
}
 @media screen and (min-width:700px) and (max-width:1200px){
    body{
	background-color: blue;
	}
}
@media screen and (max-width:700px){
    body{
	background-color: orange;
        }
}




/*
七、rem 响应式布局
*/
rem 响应式布局思想
// 一般不要给元素设置具体的宽度,但是对于一些小图标可以设定具体宽度值
// 高度值可以设置固定值,设计稿有多大,我们就严格写多大。除了登录页面这种特殊情况（铺满 & 避免滚动条），要考虑设备高度，比如在 1024*768 1280*1024 1280*800 这三个设备的范围内，以 1024*768 准，从而确定内容宽高，这是折中的做法
// 所有设置的固定值都用 REM 做单位
//   (首先在 HTML 中设置一个基准值：PX 和 REM 的对应比例,然后在效果图上获取 PX 值,布局的时候转化为 REM 值)
// JS 获取真实屏幕的宽度,让其除以设计稿的宽度,算出比例,把之前的基准值按照比例进行重新的设定,这样项目就可以在移动端自适应了

rem 与 em 有何区别
rem
// 当前页面中元素的 REM 单位的样式值都是针对于 HTML 元素的 font-size 的值进行动态计算的
em
// 表示父元素的字号的倍数。(特例：在 text-indent 属性中，表示文字宽度)

rem 有一点优势就是可以和媒体查询配合，实现响应式布局
demo 见
// https://github.com/ljianshu/Blog/issues/38
```

# 响应式-背景图

https://www.cnblogs.com/hgj123/p/6547915.html
https://www.imooc.com/article/14017
结合响应式使用

```css
/* 方法1 */
img {
  /* img是行内元素。不是块级元素。如果不给img加上block,页面上会出现一点点空隙，图片不能完全贴合 */
  display: block;
  /* 宽度如果不需要跟着容器拉伸的话，可注释下面两行 */
  width: 100%;
  max-width: 100%;
  height: 100%;
}

/* 方法2 */
.some-class {
  /* 加载背景图 */
  background-image: url(images/background-photo.jpg);

  /* 背景图垂直、水平均居中 */
  background-position: center center;

  /* 背景图不平铺 */
  background-repeat: no-repeat;

  /* 当内容高度大于图片高度时，背景图像的位置相对于viewport固定 */
  background-attachment: fixed;

  /* 让背景图基于容器大小伸缩 */
  background-size: cover;

  /* 设置背景颜色，背景图加载过程中会显示背景色 */
  background-color: #464646;
}
```

# 设备分辨率

PC 端常见分辨率 https://blog.csdn.net/weixin_30340745/article/details/97680702?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link

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

# 单位

#

# 优先级

- !important > 内联 > ID 选择器 > 类选择器 > 标签选择器
  - 尽量不要在内联样式中使用 !important

### 计算方式

- a, b, c, d
  如果有内联样式，a = 1 ，否则 a = 0，b 为 ID 选择器出现的次数，c 为类选择器、属性选择器和伪类出现的总次数，d 为标签选择器和伪元素出现的总次数
- 规则
  从左到右进行比较，数值较大的优先；如果相等，则向右比较下一位，数值较大的优先；如果 4 位数全部相等，则后声明的优先

# 选择器

### 参考

[http://www.ruanyifeng.com/blog/2009/03/css_selectors.html](http://www.ruanyifeng.com/blog/2009/03/css_selectors.html)

- 通配选择器

星号 \*， 用于匹配 html 文档内的所有元素，在搭配其他选择器使用时，会被完全忽略掉

- 属性选择器

选择器[属性条件] { 声明 }

- 属性条件

class 也可以这样操作，如 [class* = 'col-']

      - [attr] ：匹配属性名为 attr 的元素
      - [attr = value]
      - [attr ~= value] 属性值里至少有一个 = value，比如 <div attr="0 1 2"> 有3个值
      - [attr |= value] 属性值为“value”或是以“value-”为前缀
      - [attr^=value] 属性值是以"value"开头
      - [attr$=value] 属性值是以"value"结尾
      - [attr*=value] 属性值包含有"value"，不严格匹配，如 属性值 = iosAndroid，value 可为 osA、iosA 等

- 后代
  - div p {}
    匹配 div 下的 所有 p
- 子
  - 元素 1 > 元素 2 { 声明 }
    子选择器只会匹配到下一级的元素而后代选择器是匹配到所有的后代元素不管 dom 的层级有多深
- 通用兄弟
  - 位置无须紧邻，只须同层级，A~B 选择 A 元素之后【所有】同层级 B 元素
- 相邻兄弟
  - A + B
    当 B 紧跟在 A 之后，并且 A B 都是属于同一个父元素的子元素，则 B 将被选中

### 伪类

一个选择器可以同时使用多个伪类，但只能同时使用一个伪元素

- 常见
  - :hover 鼠标悬停
  - :focus 焦点
  - :first-child 表示在一组兄弟元素中的第一个元素
  - :last-child 表示在一组兄弟元素中的最后一个元素
  - :nth-child(an + b) 第 an+b 个
  #

# margin 合并

_**margin 合并是取 margin 较大一方的值，而不是相加**_

## 合并前提

[https://developer.mozilla.org/en-US/docs/Web/css/css_Box_Model/Mastering_margin_collapsing](https://developer.mozilla.org/en-US/docs/Web/css/css_Box_Model/Mastering_margin_collapsing)

- 同一 BFC 内相邻的 Block-Level 的元素
  - floating 和 absolutely positioned 的元素不会

# 清除 float

## 参考

[https://github.com/ljianshu/Blog/issues/16](https://github.com/ljianshu/Blog/issues/16)

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

# BFC

## 参考

- [https://github.com/ljianshu/Blog/issues/15](https://github.com/ljianshu/Blog/issues/15)
- [https://developer.mozilla.org/zh-CN/docs/Web/Guide/css/Block_formatting_context](https://developer.mozilla.org/zh-CN/docs/Web/Guide/css/Block_formatting_context)

## WHAT

- Block formatting context
- 块级格式化上下文
- 产生一块独立的渲染区域，只容纳块级元素。_**可理解为一个管理块级元素的容器**_

## HOW & WHY

- 如何创建
  - float 为 left | right
  - _**overflow 为 hidden | auto | scroll （常用）**_
  - display 为 table-cell | table-caption | inline-block | inline-flex | flex
  - position 为 absolute | fixed
  - 根元素
  - _**display: flow-root 可创建无副作用的 BFC ，即能避免子代 float 元素溢出的 BFC**_

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

_\*\*      \*\*_

- 内部规则
  - 内部块级元素都占一行
  - 元素逐个垂直排序，距离由 margin 决定，margin 会折叠
- 体现的特性
  - BFC 的区域不会与非同一 BFC 的 float box 重叠，即浮动不会影响其它 BFC 中元素的布局
  - 可解决父 div 高度塌陷的问题，将父 div 变成 BFC，float 的子 div 就被套入这个 BFC 了

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

      - 同一 BFC 内的 float 元素不会与其他 block 元素产生重叠

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

# 水平居中

[如何居中一个元素（终结版）](https://github.com/ljianshu/Blog/issues/29#)

### inline 元素

- `text-align: center`
- block 中是 block ，可以让里面的 block 为 inline-block

### block 元素

- `margin: {any} auto` ，不要求 margin top、bottom，只需 left right 为 auto
- `display: table` + `margin: {any} auto`
  - table 类似 block ，只是宽度为内容宽
- _**父 relative 子 absolute + transform（2D/3D 转换）**_
  - _**子向右移动父宽度的一半，再向左移动自身宽度的一半**_

```html
<div class="parent">
  <div class="child">Demo</div>
</div>
<style>
  .child {
    position: absolute;
    /* 子向右移动父宽度的一半 */
    left: 50%;
    /* 再向左移动自身宽度的一半 */
    transform: translateX(-50%);
  }

  .parent {
    position: relative;
  }
</style>
```

- flex + justify-content: center
- flex + margin auto

### 行内多个 block 元素

- flex + justify-content: center
- 全设为 inline-block，父级 text-align: center

### float 元素

#### 定宽 float

```html
<style>
  .self {
    position: relative;
    /* 向右移动父级宽度的一半 */
    left: 50%;
    /* 向左移动自身宽度的一半 */
    margin-left: -250px;
  }
</style>

<div class="parent">
  <span class="self" style="float:left;width:500px;">我是要居中的浮动元素</span>
</div>
```

#### 不定宽 float

```html
<div class="real-father">
  <div class="fake-father">
    <p class="son">float 水平居中</p>
  </div>
</div>

<style>
  .real-father {
    background-color: aquamarine;
    width: 300px;
    height: 300px;
  }
  /* .fake 这一层是披上的，没有内容的，在外一层才是原先的父元素*/
  .fake-father {
    /* 加上 float 可消除 son 的 float。是因为都脱离了文档流？ */
    float: left;
    position: relative;
    left: 50%;
  }

  .son {
    float: left;
    /* 
    fake-father 其实跟 son 是等宽等高的，
    这里移动 fake-father 宽度的一半就是移动 son 自身宽度的一半 
    */
    position: relative;
    right: 50%;
  }
</style>
```

#### 通用办法 flex

justify-content: center

### absolute 元素

_**margin auto 加上 left/right: 0 （还不能省略，很奇特，我被坑过一次）**_

```html
<div class="parent">
  <div class="child">让绝对定位的元素水平居中对齐。</div>
</div>
.parent{ position:relative; } .child{ position: absolute; /*绝对定位*/ width:
200px; height:100px; background: yellow; margin: 0 auto; /*水平居中*/ left: 0;
/*此处不能省略，且为0*/ right: 0;/*此处不能省略，且为0*/ }
```

##

# 垂直居中

### inline 元素 (单行)

inline-height 等于 height

### inline 元素 (多行)

- flex 主轴垂直后 justify-content: center

```html
<div class="parent">
  <p>
    Dance like nobody is watching, code like everybody is. Dance like nobody is
    watching, code like everybody is. Dance like nobody is watching, code like
    everybody is.
  </p>
</div>
<style>
  .parent {
    height: 140px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    border: 2px dashed #f69c55;
  }
</style>
```

- 父 table 子 table-cell，子 vertical-align: middle
  - vertical-align 用于**_ inline 元素和 table-cell 元素_** 的垂直居中

### block 元素

#### 定高 block

absolute + 负 margin

```html
<div class="parent">
  <div class="child">固定高度的块级元素垂直居中。</div>
</div>
<style>
  .parent {
    position: relative;
  }

  .child {
    position: absolute;
    top: 50%;
    height: 100px;
    /*  margin 负自身一半高度 */
    margin-top: -50px;
  }
</style>
```

#### 不定高 block

absolute + transform

#### 通用

- flex + align-items

```html
<div class="parent">
  <div class="child">未知高度的块级元素垂直居中。</div>
</div>
<style>
  .parent {
    display: flex;
    align-items: center;
  }
</style>
```

- 将父级设为 table-cell 后用 vertical-align

#

# 水平垂直居中

### 已知高宽

- absolute + 负 margin

### 已知高

- absolute + margin: auto

```css
#container {
  position: relative;
  height: 100px; /* 必须有个高度 */
}
#center {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
```

- flex/grid + margin: auto

```css
#container {
  height: 100vh; /* 必须有个高度 */
  display: grid;
  /* display: flex; */
}
#center {
  margin: auto;
}
```

- flex 的 justify-content 和 align-items

### 未知高宽

absolute  + transform

```css
#container {
  position: relative;
}

#center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

# 实现三栏布局的几种方法

[https://github.com/ljianshu/Blog/issues/14](https://github.com/ljianshu/Blog/issues/14)
**_推荐程度由高至低_**

### flex 布局

- 支持 flex 的优先 flex，支持 **IE10** 及以上

### 表格布局

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

### 浮动布局

- 先写左右浮动 div，再写中间块的 div，避免右浮动被挤到下一行。浮动 div 要记得 **清除浮动** ，避免 **容器高度塌陷**

### 绝对布局

- 三个 div 都 absolute，后代 div **在高度未知时容易出问题** ，该布局可用性较差

### 网格布局

- 最强大，但兼容性不好。IE10+上支持，而且也仅支持部分属性

## 几种常见的 CSS 布局

[https://github.com/ljianshu/Blog/issues/40](https://github.com/ljianshu/Blog/issues/40)

# CSS 预处理器

## 参考

[再谈 CSS 预处理器](https://efe.baidu.com/blog/revisiting-css-preprocessors/)

## What

CSS 扩展语言，编译为 CSS。有 less、sass、stylus，是我会选 stylus

## Benifit

- 添加编程特性，如 **变量** 、 **函数**
- 减少书写重复的选择器

## Point

- 基本语法
- 嵌套
- 变量
- [@import ]()
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
