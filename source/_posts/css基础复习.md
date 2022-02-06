---
title: css基础复习
date: 2022-01-30 11:48:46
tags:
- css
categories:
- 前端
---
### 基本概念

`css`的基本定位和布局机制是**文档流**，默认自上而下，从左到右

### 块级元素

`div、ul、li、p、table、h1` 等元素，他们的默认`display`默认是`block、table、list-item`

### 内联元素（行内元素）

如`span、a、em、i、img、td`等，这些元素的display值默认是`inline、inline-block、inline-table、table-cell`等。

### 关于 `width: auto`  和 `height: auto`

`width`、`height`的默认值都是`auto`。

**对于width**

块级元素：流体布局之下`width: auto`自适应撑满父元素宽度。这里的撑满并不同于`width: 100%`的固定宽度，而是像水一样能够根据`margin`不同而自适应父元素的宽度。

内联元素：`width: auto`则呈现出包裹性，即由子元素的宽度决定。

**对于height**

无论内联元素还是块级元素，`height: auto`都是呈现包裹性，即高度由子级元素撑开。

父元素`height: auto`会导致子元素`height: 100%`百分比失效。

`css`的属性非常有意思，正常流下，如果块级元素的`width`是个固定值，`margin`是`auto`，则`margin`会撑满剩下的空间；如果`margin`是固定值，`width`是`auto`，则`width`会撑满剩下的空间。这就是流体布局的根本所在。

### `css`权重

| 权重值  | 选择器                                                       |
| ------- | ------------------------------------------------------------ |
| 1,0,0,0 | 内联样式：style=""                                           |
| 0,1,0,0 | ID选择器：`#idName{...}`                                     |
| 0,0,1,0 | 类、伪类、属性选择器：`.className{...}` / `:hover{...}` / `[type="text"] ={...}` |
| 0,0,0,1 | 标签、伪元素选择器：`div{...}` / `:after{...}`               |
| 0,0,0,0 | 通用选择器（*）、子选择器（>）、相邻选择器（+）、同胞选择器（~） |

### 盒模型（盒尺寸）

元素的内在盒子是由`margin box`、`border box`、`padding box`、`content box`组成的，这四个盒子由外到内构成了盒模型。

IE模型： `box-sizing: border-box`  此模式下，元素的宽度计算为`border+padding+content`的宽度总和。

w3c标准盒子模型： `box-sizing: content-box` 此模式下，元素的宽度计算为`content`的宽度。

由于`content-box`在计算宽度的时候不包含`border pading`很烦人，而且又是默认值，业内一般采用以下代码重置样式：

```javascript
:root {
    box-sizing: border-box;    
}
* {
    box-sizing: inherit;
}
```

### 内联盒模型

![Image text](/images/内联盒模型.png)

### 参考

作者：幻灵尔依
链接：https://juejin.cn/post/6844903894313598989
来源：稀土掘金
