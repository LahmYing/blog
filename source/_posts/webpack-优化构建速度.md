---
title: webpack 优化构建速度、前端性能
date: 2021-12-31 16:20:23
tags: [webpack]
---

{% asset_img WechatIMG155.png %}

<!-- toc -->

# 补充

## 参考

https://www.yuque.com/wangpingan/cute-frontend/gdie85#VFhoR
https://www.yuque.com/wangpingan/cute-frontend/lptos6

## 记录缓存 splitChunks 行为

如果您有一个复杂的代码分割设置，并希望确保分割部分获得正确的缓存行为，则记录尤其有价值。最大的问题是维护记录文件
如果你改变 webpack 处理模块 id 的方式，可能存在的记录仍然会被考虑在内!如果想使用新的模块 ID 方案，还必须删除记录文件

```js
// vue.config.js
config.recordsPath(path.join(__dirname, "records.json"));
```

### 参考

https://survivejs.com/webpack/optimizing/separating-runtime/
https://webpack.docschina.org/configuration/other-options/#recordspath

## 精准 resolve， 减少 resolve 的解析

```js
resolve: {
  modules: [
    path.resolve(__dirname, 'node_modules'), // 使用绝对路径指定 node_modules，不做过多查询
  ],
  // 删除不必要的后缀自动补全，少了文件后缀的自动匹配，即减少了文件路径查询的工作
  // 其他文件可以在编码时指定后缀，如 import('./index.scss')
  extensions: [".js"],
  // 避免新增默认文件，编码时使用详细的文件路径，代码会更容易解读，也有益于提高构建速度
  mainFiles: ['index'],
},
```

## 精准 loader 的范围

```js
rules: [
  {
    test: /.jsx?/,
    include: [
      path.resolve(__dirname, 'src'),
      // 限定只在 src 目录下的 js/jsx 文件需要经 babel-loader 处理
      // 通常我们需要 loader 处理的文件都是存放在 src 目录
    ],
    use: 'babel-loader',
  },
  // ...
],
```

## webpack 优化前端性能

按需加载，分 chunk 和加哈希值协助浏览器缓存行为，静态资源和公共库上 CDN...
更多相关优化见 http://lahmying.top/2021/09/30/%E4%BC%98%E5%8C%96/
