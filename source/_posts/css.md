---
title: css
date: 2021-08-26 19:29:26
tags: [css]
---

<!-- toc -->

# js css 共享变量

[https://segmentfault.com/a/1190000018795983?utm_source=tag-newest](https://segmentfault.com/a/1190000018795983?utm_source=tag-newest)
[https://www.npmjs.com/package/sass-resources-loader](https://www.npmjs.com/package/sass-resources-loader)

# CSS 逻辑属性

## -inline 和 -block

margin/padding/border-inline/block

`-inline` -left 加 -right，跟书写方向有关，`margin-inline: 5px 10px`, 从左到有 margin-left 为 5px，右到左为 10px

`-block` -top 加 bottom，其余同上

eg ` border-block: 8px solid blue;`

## inline-size 和 block-size

```js
width	inline-size
max-width	max-inline-size
min-width	min-inline-size
height	block-size
max-height	max-block-size
min-height	min-block-size
```

## Position

```js
top	inset-block-start
bottom	inset-block-end
left	inset-inline-start
right	inset-inline-end
top and bottom	inset-block
left and right	inset-inline
```

# 书写方向

CSS

`writing-mode: horizontal-tb | vertical-rl | vertical-lr | sideways-rl | sideways-lr`

HTML

尽量使用 HTML 属性以防 CSS 无法加载

`<html dir="rtl">`

# CSS 风格

## SMACSS

[SMACSS](https://link.segmentfault.com/?url=https%3A%2F%2Fsmacss.com%2F)

- Base
- Layout
- Module
- State
- Theme

## CSS module

以 webpack 为例，使用 css-loader 就可以实现 CSS module

## styled-components

样式组件，all-in-js 的理念，不好阅读

```js
import styled from "styled-components";
import styles from "./style.less";

const Wrapper = styled(div)`
  border: 1px dashed ${(props) => props.color};
  width: 100%;
`;

const Header = (props) => {
  return (
    <div>
      {/* 直接看 jsx，看不出来 Wrapper 的原始标签是 div */}
      <Wrapper color="#000">使用 styled-component </Wrapper>
      <div className={styles.Wrapper}>使用 CSS Modules</div>
    </div>
  );
};
```

# PostCss

转译、兼容前缀
