---
title: vue SFC 编译为 .js（使用gulp）
date: 2021-10-09 14:34:05
tags: [gulp, vue]
category: [vue]
---

<!-- toc -->

# vue SFC 编译为 .js

https://www.gulpjs.com.cn/docs/getting-started/quick-start/
https://www.npmjs.com/package/gulp-vue-compiler
https://www.npmjs.com/package/babel-env
https://github.com/vuejs/vue-component-compiler/issues/28

cmd

```sh
# gulp 命令行工具
yarn global add gulp-cli
yarn add gulp gulp-vue-compiler babel-env

# 执行 task
gulp vue-compile
```

gulpfile.js

```js
let vueCompiler = require("gulp-vue-compiler");
let gulp = require("gulp");

gulp.task("vue-compile", function () {
  return gulp
    .src("src/components/**/*.vue")
    .pipe(
      vueCompiler({
        newExtension: "js", // *.vue => *.js
        babel: {
          babelrc: false,
          presets: [
            [
              "env",
              {
                modules: false, // Keep ES modules in 'script' tag
                targets: {
                  browsers: ["> 1%", "last 2 versions"],
                },
              },
            ],
            "stage-3",
          ],
        },
      })
    )
    .pipe(gulp.dest("./dist/"));
});
```
