---
title: SEO
date: 2021-09-06 16:14:30
tags: [SEO]
category: []
---

<!-- toc -->

# 参考

[https://futu.im/article/2019-seo/](https://futu.im/article/2019-seo/)

# 搜索引擎

- 抓页面的静态内容，抓取不了动态生成的内容
  - 动态如 `<p> {{ 变量 }} <p>`，前端渲染基本都是动态生成，后端渲染就是完全静态的 html
- 工作流程
  - 从网站的 URL 进入页面
    - 1.抓取页面 HTML 代码进数据库
    - 2.处理页面的数据文字，生成索引

# SEO 优化

- 目前，移动优先是 SEO 优化的重点

## 优化方式

### 页面内的静态内容

- 关键字加粗
- 页面布局结构扁平化
- 语义化
- 标题权重划分

### robots

#### 使用 robots.txt

- 告知搜索引擎“你可以对我这个站点上的哪些 URL 进行跟踪并索引”
- 所有主要的搜索引擎都能读取并遵守它，一天刷新好几次
- 存放位置
- 域名/robots.txt
- 语法
  - [https://baijiahao.baidu.com/s?id=1645704782665020815&wfr=spider&for=pc](https://baijiahao.baidu.com/s?id=1645704782665020815&wfr=spider&for=pc)
- 用工具生成 robot.txt ，且可在 txt 中添加 sitemap

#### `<meta name=’robots’ content=’noindex,follow’ />`

- 该 meta tag 只有少数引擎能识别，尽量使用 robots.txt
- 允许跟踪页面中的链接继续抓取：是 follow 否 nofollow
- 允许索引: 是 index 否 noindex

### 提供网站 sitemap，用工具生成即可，搜 “生成 sitemap”

### URL 适当使用「nofollow」，减少不必要的抓取工作

- `<a href="" rel="nofollow">`

### 尽量不使用单页面，增加页面导航

可使用预渲染插件

### URL 链接优化

### 提高网站打开速度

# 搜索引擎对 HTML tag 的评分

```javascript
内部链接文字：10分
标题title：10分
域名：7分
H1,H2字号标题：5分
每段首句：5分
路径或文件名：4分
相似度（关键词堆积）：4分
每句开头：1.5分
加粗或斜体：1分
文本用法(内容)：1分
title属性：1分 (注意不是title>, 是title属性, 比如a href=… title=”)
alt标记：0.5分
Meta描述（Description属性）：0.5分
Meta关键词（Keywords属性）：0.05分
```

# 查看指定站点搜索结果

- `site:xxx.com`
