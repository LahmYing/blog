---
title: cookie-session-token
date: 2021-08-27 09:45:34
tags: [网络运维, cookie]
category: [网络运维]
---

<!-- toc -->

# Cookie、Session、Token、JWT

https://juejin.cn/post/6844904034181070861

## 常用鉴权方式

在 web 前后端分离的项目中，基本都是用 token 标识用户

session 这种东西在传统的前后端耦合项目比较很常用

维持 session 会消耗内存

### Cookie 和 Session

Cookie - session_id - Session

服务器（生成） - Cookie - 浏览器（保存）

### JSON Web Token

session_id 匹配 user

#### 场景

用户在同一公司下的站点 A 登录后，可免登录该公司的其他站点

#### 重点

![jwt](https://33e9-dev-upload.oss-cn-beijing.aliyuncs.com/executeTask/image/f5/f52c465456ed411547aadfa8c2bbc3a9.jpg)

<!--<img src="https://33e9-dev-upload.oss-cn-beijing.aliyuncs.com/executeTask/image/f5/f52c465456ed411547aadfa8c2bbc3a9.jpg">-->

## 多服务、分布式下的 session 共享

有以下方案

`login_session 放在各服务` 变动时广播复制到其他服务

`login_session 放在某个服务` 在 A 服务登录，A 中生成 login_session，请求其他服务时先去 A 服务验证登录状态

`所有 session 放缓存区` 将所有服务的 session 缓存在 Redis 集群

`session 放数据库`

## 关闭浏览器，session 消失？

关闭浏览器 - cookie 丢失（看有效期） - session id 丢失 - 匹配不了服务器上对应的 session
