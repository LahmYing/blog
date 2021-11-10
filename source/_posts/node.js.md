---
title: node.js
date: 2021-09-17 15:55:30
tags: [node.js]
---

<!-- toc -->

# node.js web 框架

koa.js,egg.js,express.js 三者有什么区别？
https://www.zhihu.com/question/391604647/answer/1327741879

# node.js 支持部分 es6 语法

安装 es-checker 可查看支持程度

```sh
lanying@lanyingdeMacBook-Pro _not-business % yarn global add es-checker
yarn global v1.22.11
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
warning " > ts-node@10.2.1" has unmet peer dependency "@types/node@*".
[4/4] 🔨  Building fresh packages...

success Installed "es-checker@1.4.2" with binaries:
      - es-checker
✨  Done in 2.37s.
lanying@lanyingdeMacBook-Pro _not-business % es-checker

ECMAScript 6 Feature Detection (v1.4.2)

Variables
  √ let and const
  √ TDZ error for too-early access of let or const declarations
  √ Redefinition of const declarations not allowed
  √ destructuring assignments/declarations for arrays and objects
  √ ... operator

Data Types
  √ For...of loop
  √ Map, Set, WeakMap, WeakSet
  √ Symbol
  √ Symbols cannot be implicitly coerced

# ......(后续不列举了)
```
