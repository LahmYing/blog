---
title: GraphQL
date: 2021-09-09 10:34:53
tags: [接口]
---

<!-- toc -->

# 各端职责与协作

- 前后端讨论定义 schema
  - schema 里是`含类型的业务字段 + 业务字段进一步的聚合`
- 后端需要提供 `schema 形状的接口`
- 前端撰写 `schema 查询和解析函数`

# 对比以往基于 RESTful 风格接口的协作

对于前端，只不过是提前把 `业务model` 写出来，以前是在得到响应后开始整理 `业务model`，比如 `this.list = res.data.list`

对于后端，则改动很大

- 写法大变动，要搭建 GraphQL 服务
- 以前是一个需求 `新增一个接口`，现在则 `新增一个 schema`，可能更多的是在原有的 `schema` 上进行改动（相比新增接口多了点风险，且对业务能力要求更高，但有助于提炼业务）

# 写法

## schema

```typescript
// 类型字段，假设位于 a 文件
type Query {
  me: User
}

type User {
  id: ID
  name: String
}

// 对应的解析函数，假设位于 b 文件
function Query_me(request) {
  return request.auth.user;
}

function User_name(user) {
  return user.getName();
}
```

## 查询和 response

SQL 之类的查询和 response

```json
// 查询
{
  me {
    name
  }
}

// response
{
  "me": {
    "name": "Luke Skywalker"
  }
}
```
