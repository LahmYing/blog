# Hexo blog

```json
"scripts": {
    "build": "hexo generate",
    "clean": "hexo clean",
    "deploy": "hexo deploy",
    "serve": "hexo server",
    // serve 前置钩子，格式化 md 文件
    "preserve": "yarn format",
    "format": "prettier -w ./source/**/*.md",
    "format-generated": "prettier -w ./public/*/**/*.html"
  }
```
