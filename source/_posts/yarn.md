---
title: yarn
date: 2021-08-27 17:31:52
tags: [依赖管理]
category: [依赖管理]
---

<!-- toc -->

# 镜像

查询当前
`yarn config get registry`
设为淘宝镜像
`yarn config set registry `[`http://registry.npm.taobao.org/`](http://registry.npm.taobao.org/)
设为默认的官方镜像
`yarn config set registry `[`https://registry.yarnpkg.com`](https://registry.yarnpkg.com)

# 添加包

`yarn add [package]`
`yarn add [package]@[version]`
`yarn add [package]@[tag]`

### 全局添加

`yarn global <add/bin/list/remove/upgrade>`
global 是一个必须跟在 yarn 后面的命令。输入 `yarn add global package-name` 会把名为 global 和 package-name 的包添加到本地，而非全局添加 package-name

### 添加到 devDependencies、peerDependencies 和 optionalDependencies 类别

`yarn add [package] --dev`
`yarn add [package] --peer`
`yarn add [package] --optional`

# 升级包

指定包名称升级指定包
不指定包名称升级所有包
`yarn upgrade [package]`
`yarn upgrade [package]@[version]`
`yarn upgrade [package]@[tag]`

# 移除包

`yarn remove [package]`

# 安装包

`yarn`
`yarn install`

# 列出已安装的包

默认情况下，所有的包及其所依赖的包，都会被显示

### 限制包的深度

`yarn list`
`yarn list --depth=0`

### 指定字眼

`yarn list --pattern`
`yarn list --pattern gulp`
`yarn list --pattern "gulp|grunt" --depth=1`

# 显示包信息

某版本的包的信息
`yarn info [package]@[version]`
项目中某包的版本
`yarn info [package] version`

# 运行

`yarn run [script] [脚本参数]`
`yarn run test`
`yarn run test -o --watch`
如果你不指定一个脚本给 yarn run 命令，run 命令会列出包里所有可运行的脚本
`yarn run`

# 列出运行时可用的环境变量

`yarn run env`
如果想覆盖此命令，可以在 package.json 中定义自己的 "env" 脚本

# yarn 升级

```bash
sudo -s # mac 进入管理员模式

npm uninstall -g yarn # 先卸载

npm install yarn@1.3.2 -g # 安装指定版本

npm install yarn@latest -g # 安装最新版本
```
