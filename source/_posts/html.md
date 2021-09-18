---
title: html
date: 2021-08-26 20:42:26
tags: [html, 浏览器]
---

<!-- toc -->

# 请求头不发送 Referer

https://segmentfault.com/q/1010000000123441
https://blog.csdn.net/qq_37236241/article/details/119330230

```js
openWithoutReferer(url) {
    window.open(
      "javascript:window.name;",
      "<script>location.replace('" + url + "')</scr" + "ipt>"
    );
}
```

# template 不与数据结构耦合

除非动态生成的&有较强一致性的如表格，一般建议平铺，嫌重复代码多的话就将其塞入小组件，外面引用小组件看起来就清爽些了

# BOM(browser object model)

BOM 包含 5 个东西

- location 管理 URL
- navigator 管理浏览器
- history 管理历史记录
- screen 管理屏幕
- window 管理浏览器所有的东西

# 浏览器渲染流程

{% asset_img 浏览器渲染流程.png 800 600 浏览器渲染流程 %}

- CSS 解析不会影响 HTML 解析，但会阻塞 js 的加载（即要等前面的 CSS 加载完）
- js 会阻塞 HTML 和 CSS 的解析

https://segmentfault.com/a/1190000010298038

https://blog.csdn.net/lucaslow/article/details/78307396

# pre 标签

```html
<pre
  style="
                    text-align: left;
                    appearance: none;
                    white-space: pre-wrap;
                    width: 100%;
                    letter-spacing: normal;
                    tab-size: 2;
                    font-size: 14px;
                    font-family: 'Microsoft YaHei';
                    line-height: 1.5;
                    word-break: break-all;
                    "
>
营销内容
</pre>
```

# HTML5 支持 MathML

# HTML5 拖放

拖放是 HTML5 标准的组成部分

# 获取地理位置

`navigator.geolocation.getCurrentPosition(successFuc, errorFuc, options)`

https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/getCurrentPosition

在桌面浏览器使用 geolocation 会遇到网络阻塞问题 （国内政策）, 在移动端是完全可以的

# SVG

用网页打开

# embed 标签

`<embed src="https://33e9-dev-upload.oss-cn-beijing.aliyuncs.com/executeTask/image/b9/b967216c5ca077f307785cdf8b817fd4.jpg" width="640" height="480">`

embed 标签只支持 PC 端，移动端无效？
