---
title: CSS BFC
date: 2022-02-06 21:49:37
tags:
- css
categories:
- 前端
---
关于BFC

#### BFC概念

`BFC` 全称：`Block Formatting Context`， 名为 "块级格式化上下文"。

`W3C`官方解释为：`BFC`它决定了元素如何对其内容进行定位，以及与其它元素的关系和相互作用，当涉及到可视化布局时，`Block Formatting Context`提供了一个环境，`HTML`在这个环境中按照一定的规则进行布局。

简单来说就是，`BFC`是一个完全独立的空间（布局环境），让空间里的子元素不会影响到外面的布局。那么怎么使用`BFC`呢，`BFC`可以看做是一个`CSS`元素属性

#### 触发BFC

- 根元素，即`html``
- ``overflow`不为`visible`
- `float`的值不为`none`
- `display`的值为`inline-block`、`table-cell`、`table-caption`
- `position`的值为`absolute`或`fixed`

作者：蛙人
链接：https://juejin.cn/post/6950082193632788493
来源：稀土掘金

