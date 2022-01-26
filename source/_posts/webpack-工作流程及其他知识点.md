---
title: webpack 工作流程及其他知识点
date: 2022-01-04 14:11:38
tags: [webpack]
---

{% asset_img webpack-workflow-and-other.png %}

<!-- toc -->

# 补充

## 常用 loader

（1）babel-loader：把 es6 转成 es5；
（2）css-loader：加载 css，支持模块化，压缩，文件导入等特性；
（3）style-loader：把 css 代码注入到 js 中，通过 dom 操作去加载 css；
（4）eslint-loader：通过 Eslint 检查 js 代码；
（5）image-loader：加载并且压缩图片晚间；
（6）file-loader：文件输出到一个文件夹中，在代码中通过相对 url 去引用输出的文件；
（7）url-loader：和 file-loader 类似，文件很小的时候可以 base64 方式吧文件内容注入到代码中。
（8）source-map-loader：加载额外的 source map 文件，方便调试。

## 常用 plugin

（1）uglifyjs-webpack-plugin：通过 UglifyJS 去压缩 js 代码；
（2）commons-chunk-plugin：提取公共代码；
（3）define-plugin：定义环境变量。

## 热更新

{% asset_img 热更新.jpg %}

webpack 给 bundle **_加塞_** 了两个文件，最终浏览器运行时多了两个文件：

- webpack-dev-server/client/index.js，用来跟 webpack server **_进行 websocket 通信_** 的
- webpack/hot/dev-server.js，用来 **_热替换模块或 reload 浏览器_** 的

### 简略流程

- webpack watch 文件变化，文件被重新编译打包并存到内存
- webpack server 端和浏览器端实现了 websocket 通信，浏览器接到文件变化的通知，然后通过 ajax 向 webpack server 端请求最新 module
- 浏览器端的 webpack/hot/dev-server.js 进行热更新或 reload 浏览器

## 多入口应用场景

多个类似的小项目复用一套公共库，比如用 Vue2 和 iview 上了一个表单为主的项目 A，此时又有需求，准备基于 A 的业务从另一个角度产出几张表单，称为 B 项目

### 为什么不用分支来区分类似项目

- 发布时你还要切分支
- 如果开发时发现一个公共库的 bug, 你修复后还要分别推到各项目分支

## 模块联邦

**_webpack 5_** 支持这一功能

### 作用

你可以在多个应用之间共享、复用 **_业务代码（比如模块甚至页面级组件）、公共库甚至应用_**

### 简单说明

先说几个概念/定义

- bundle：你的项目经 webpack 编译打包后的所有产物，认为是一个 bundle
- 远程模块：不属于当前构建的，在运行时从所谓的容器加载。你可以这样理解：通过 `import() 或 require.ensure 或 require([...])` **_动态加载_** 的就是远程模块
- 容器：所有远程模块都从容器加载而来。每个 bundle 都充当一个容器

运行时，容器可以暴露模块给其他容器消费/使用，比如 bundle A 包括了组件库 `"view-design": "^4.6.1"` 和某个页面级组件 `CompanyDesc.vue` 并暴露给 bundle B，那 bundle B 的源代码就不用装 `"view-design": "^4.6.1"`，也不用去 copy 一份 `CompanyDesc.vue` 了

但切记要先启动 A

### 参考

https://webpack.docschina.org/concepts/module-federation/
https://blog.csdn.net/qq_29438877/article/details/105672029
