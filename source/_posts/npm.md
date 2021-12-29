---
title: npm
date: 2021-08-26 20:38:03
tags: [依赖管理]
category: [依赖管理]
---

<!-- toc -->

# npm install 版本号

`^15.2.1` match `^15.x.x`

`~15.2.1` match `^15.2.x`

`^0.2.1` match `^0.2.x`

从左边非 0 版本号开始

# 切换 Node 版本

## n

n 是一个 npm 包，n 依赖于 node 环境

安装 n `sudo npm install n -g`

### 安装指定版本 node

安装稳定版
`sudo n stable`

安装最新版
`sudo n latest`

安装指定版本
`sudo n 10.16.3`

选择版本或升降级
`sudo n 版本号`

删除某个版本
`sudo n rm 12.15.3`

以指定的版本来执行脚本
`n use 10.13.0 test.js`

查看帮助命令
`n help`

## nvm

nvm 是一个独立软件，不依赖于 node 环境。如果不会频繁切换 node 版本，还是推荐 n，所以此处不再赘述

## 管理 node 版本，选择 nvm 还是 n？

[管理 node 版本，选择 nvm 还是 n？](https://www.cnblogs.com/shengulong/p/9343172.html)

# 查看当前项目的所有 npm 脚本命令

`npm run`

npm 脚本就是 Shell 脚本，因为可以使用 Shell 通配符

# 钩子

npm 脚本有 pre 和 post 两个钩子。举例来说，build 脚本命令的钩子就是 prebuild 和 postbuild

`npm run build` = `npm run prebuild && npm run build && npm run postbuild`

# 执行顺序

- 并行 &
- 串行 &&

# npm 切换版本

`npm install -g npm@版本号`
