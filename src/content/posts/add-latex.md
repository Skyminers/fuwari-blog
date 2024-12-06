---
published: 2024-12-06
title: Astro 添加 LaTeX 支持
tags: [Blog]
category: 方法记录
description: 记录了为 astro 添加 LaTeX 支持的方法
---

参考了：[alexafazio](https://alexafazio.dev/blog/render-latex-in-astro/) 这篇文章。

显示效果实例：

| 源码                | 显示效果            |
| ------------------- | ------------------- |
| `$x^2$`             | $$x^2$$             |
| `$\frac{1}{2}$`     | $$\frac{1}{2}$$     |
| `$\sqrt{2}$`        | $$\sqrt{2}$$        |
| `$\int_0^1 x^2 dx$` | $$\int_0^1 x^2 dx$$ |

## 1. 安装依赖

打开项目根目录，首先安装依赖：

```bash
npm install remark-math
npm install rehype-katex
```

## 2. 配置

在项目根目录下的`astro.config.js`中添加配置,添加后的配置文件示例如下：

```js
// astro.config.js
import remarkMath from "remark-math";
import rehypeKatex from "rehype-katex";

export default defineConfig({
  ..., // 其他配置
  markdown: {
    remarkPlugins: [remarkMath], // 添加 remark-math 插件
    rehypePlugins: [rehypeKatex],// 添加 rehype-katex 插件
  },
});
```

在`./components/Header.astro`的`<header>`中第一行添加如下标签：

```HTML
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.15.1/dist/katex.css" integrity="sha384-WsHMgfkABRyG494OmuiNmkAOk8nhO1qE+Y6wns6v+EoNoTNxrWxYpl5ZYWFOLPCM" crossorigin="anonymous">
```

该行标签需要添加到页面的 HTML 中才可以正常显示共识，如果`Header.astro`文件无法找到的话，就需要自行寻找被加入到内容页面的文件。实在不行可以在每一个使用 LaTeX 公式 md 文件内加入这行代码，也可以成功显示，只是略显麻烦一些。

## Done!

配置完成，现在可以使用 LaTeX 了！

## 补充

目前的配置下多行公式在 Dark Mode 下可能会显示成灰色，很难认清，可以在`/layouts/PostDetails`文件中的`<style>`标签中添加如下代码：

```css
/* This is fixing the dim color of katex in dark mode */
.prose {
  color: var(--color-text-base);
}
```

可以使多行公式的颜色正常显示，方便阅读。
