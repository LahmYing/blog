---
title: all_day_log
date: 2021-04-22 16:22:12
tags:
---

<!-- toc -->

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

# 老项目（无框架+gulp）

### package.json、.gitignore

### 本地服务 && 保存即刷新页面

监控到文件变化，就自动重启服务

### 多 html（路由切换）

老项目中切换不同 html ，跟 Vue 中根据路由加载不同的 .vue 组件，是一个意思，都是切换 URL 和加载该页面所需的资源（ css js 静态资源 ）

### 编译压缩 js、css、html

定义的 js 方法名、css 类名等等保持不变，尽可能减少空格和字节，比如 **_let t = this_**

### 复制到 dist 文件夹

images、**_库文件_**、压缩后的 css、js、html

# git log

```zsh
git log --graph --oneline --decorate
git log --graph --decorate
```

# 覆盖组件库 ivew 样式

main.js 中。 **_跟 CSS 文件的引用次序有关_**

```js
import "view-design/dist/styles/iview.css";
// 定制主题色
import "./assets/customTheme/index.less";
```

# 样式覆盖、Vue 的 scoped 和 /deep/

{% asset_img Vue的scoped和deep.png 1000 1000 image desc %}

# 匹配文件

https://github.com/isaacs/node-glob
https://github.com/isaacs/minimatch

## 像 shell 一样匹配文件

shell 本身支持匹配文件，node-glob 包使我们可以在 js 中像 shell 一样去匹配文件

### 语法

```
<!-- 列出匹配的文件 -->
$ ls src/**/*.js
```

`*` 匹配任意长度任意字符
`?` 匹配任意单个字符
`[list]` 匹配指定范围内（list）任意单个字符，也可以是单个字符组成的集合
`[^list]` 匹配指定范围外的任意单个字符或字符集合
`[!list]` 同[^list]
`{str1,str2,...}` 匹配 srt1 或者 srt2 或者更多字符串，也可以是集合

# this

非箭头函数：会变动，运行时确定，可用 call() 改写后确定

箭头函数：定义时确定，本身没绑定 this，取父级的 context，最近的 {} 即为父级

# JSONP

- html

```html
<script src="http://example.com/data.json"></script>
```

- Service

```js
someCallback(responeObj);
```

- Client

```js
// Client 定义的函数要与 Service 的同名
window.someCallback = function (responeObj) {
  //
};
```

## jQuery 有处理方案

`jQuery.getJSON("http://example.com/data.json?callback=?", function(result){ // 处理返回结果的相关逻辑 });`

jQuery 将上面 URL 中最后的问号替换为一个由它创建的随机命名的临时函数。服务器 会获取这个 callback 参数，使用这个名字作为回调函数名称返回给客户端。

# require(module)

requirejs 使用的是 AMD 规范，AMD 提倡的是依赖前置，也就是说不管你的 require 写在什么位置，都会提前加载，比如：

```js
if (browser.desktop && browser.msie && browser.versionNumber < 9) {
  require("selectivizr");
}
```


# npm install 版本号

`^15.2.1` match `^15.x.x`

`~15.2.1` match `^15.2.x`

`^0.2.1` match `^0.2.x`

从左边非 0 版本号开始




