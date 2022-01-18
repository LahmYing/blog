---
title: html及浏览器
date: 2021-08-26 20:42:26
tags: [html, 浏览器]
---

<!-- toc -->

# popstate 事件-监听浏览器历史的改变

https://developer.mozilla.org/zh-CN/docs/Web/API/Window/popstate_event

调用 history.pushState()或 history.replaceState()不会触发 popstate 事件。
只有在做出浏览器动作时，才会触发该事件，如用户点击浏览器的回退按钮（或者在 Javascript 代码中调用 history.back()或者 history.forward()方法）

# 卸载（unload）文档之前向 web 服务器发送数据（埋点）

Navigator.sendBeacon()

```js
// onunload 特性(乃至 unload 事件本身) 并非使用 sendBeacon()的正确途径，
// 要调用 sendBeacon() 接口，应当使用 visibilitychange 和 pagehide 事件
window.addEventListener("unload", logData, false);

function logData() {
  let apiUrl = "";
  navigator.sendBeacon(apiUrl, analyticsData);
}
```

# pagehide event

当浏览器在显示与会话历史记录不同的页面的过程中隐藏当前页面时，**_正常跳转、点击浏览器前进后退都会触发_**，离开页面不建议使用 unload beforeunload event

```js
window.addEventListener("pagehide", hide);

function hide(e) {
  debugger;
  console.log(e);
}
```

# document.readyState

- loading（正在加载）
- interactive（可交互）
  文档已被解析，"正在加载"状态结束，但是诸如图像，样式表和框架之类的子资源仍在加载
- complete（完成）
  文档和所有子资源已完成加载

# 上下文切换

当使用 浏览器内置的 addEventListener() 时，上下文切换为目标 HTML 元素

```js
let a = new (function () {
  this.appName = "wem";
  document.body.addEventListener(
    "click",
    function (e) {
      // 上下文发生改变，因此 appName 是 undefined
      console.log(this);
      console.log(this.appName);
    },
    false
  );
})();
```

# useCapture

```js
// useCapture: 是否使用捕获模式
div.addEventListener(
  "blur",
  function () {
    /* ... */
  },
  useCapture
);
```

# 执行顺序

一直捕获（触发所有父级别的捕获类型的事件），直到所触发的子元素，后冒泡。若该子元素上同时绑定了 捕获事件 A 和 冒泡事件 B，AB 的执行顺序同脚本中的顺序

阻止冒泡 e.stopPropagation()
阻止默认行为 e.preventDefault()

# currentTarget 和 target

currentTarget 总是指向事件绑定的元素，而 Event.target 则是事件触发的元素

# 坐标相关

clientX：当鼠标事件发生时（不管是 onclick，还是 omousemove，onmouseover 等），鼠标相对于浏览器（这里说的是浏览器的有效区域）x 轴的位置；
clientY：当鼠标事件发生时，鼠标相对于浏览器（这里说的是浏览器的有效区域）y 轴的位置；
screenX：当鼠标事件发生时，鼠标相对于显示器屏幕 x 轴的位置；
screenY：当鼠标事件发生时，鼠标相对于显示器屏幕 y 轴的位置；
offsetX：当鼠标事件发生时，鼠标相对于事件源 x 轴的位置
offsetY：当鼠标事件发生时，鼠标相对于事件源 y 轴的位置
pageX，pageY：事件发生时相对于页面（如 viewport 区域）的坐标

# 内容安全策略（CSP）

https://blog.csdn.net/qq_37943295/article/details/79978761

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

{% asset_img 浏览器渲染流程.png %}

- CSS 解析和 HTML 解析是互不干扰的，但 CSS 解析会阻塞 js 的加载，因为浏览器在执行 js 前要确保之前的 CSS 规则树构建完毕。比如浏览器按顺序加载 a.css、b.js、c.css，那么 b.js 要等 a.css 阶段的 CSS 规则树构建完毕才会执行
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

# HTML5 拖放

拖放是 HTML5 标准的组成部分

# 获取地理位置

`navigator.geolocation.getCurrentPosition(successFuc, errorFuc, options)`

https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/getCurrentPosition

在桌面浏览器使用 geolocation 会遇到网络阻塞问题 （国内政策）, 在移动端是完全可以的

# SVG

用网页打开

# embed 标签

```html
<embed
  src="https://33e9-dev-upload.oss-cn-beijing.aliyuncs.com/executeTask/image/b9/b967216c5ca077f307785cdf8b817fd4.jpg"
  width="640"
  height="480"
/>
```
