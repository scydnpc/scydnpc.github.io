---
title: (转载) lodash源码 chunk
date: 2021-12-20
categories:
- 前端
tags:
- javascript
- lodash
---
## _.chunk(array, [size=1])
> 介绍：将数组（`array`）拆分成多个 `size` 长度的区块，并将这些区块组成一个新数组。 如果array 无法被分割成全部等长的区块，那么最后剩余的元素将组成一个区块。  
参数：  
`array` (Array): 需要处理的数组  
`[size=1]` (number): 每个数组区块的长度

#### 用途
`chunk` 函数在前端可以用来缓解一些性能问题。例如大量的 `DOM` 操作，可以分块让浏览器在空闲的时候处理，避免页面卡死。如下面的代码，向页面中插入大量的 `DOM` 。
```javascript
const arr = [] // 1万条数据
const chunks = _.chunk(arr, 100)

const append = function () {
  if (chunks.length > 0) {
    const current = chunks.pop()
    current.forEach(item => {
      const node = document.createElement('div')
      node.innerText = item
      document.body.appendChild(node)
    })
    setTimeout(append, 0)
  }
}

append()
```
#### 依赖
```javascript
import slice from './slice.js'
```
#### 原理
`chunk` 的原理归结起来就是切割和放置。  

`chunk` 最后返回的结果如`[[1],[1],[1]]`的形式，放置就是将切割下来的块放置到数组容器中。  

那要怎样切割呢？

因为指定了大小，因此切割跟切蛋糕很像，参数 `size` 是尺子，测好每块的长度，`slice` 函数是刀， 将数组一块一块切出来。  例如有 `[1,2,3,4,5]` 这个数组，`size` 指定为 `2`，则第一次切割会得到 `[1,2]` 的块，第二次切割得到 `[4,5]`，剩下的是 `[5]` 。这个数组最终会被切为三块。
#### 源码总览
```javascript
function chunk(array, size) {
  // 确保 length 存在 且 size 比 1 大
  size = Math.max(size, 0)
  const length = array == null ? 0 : array.length
  // 否则 返回空数组
  if (!length || size < 1) {
    return []
  }
  let index = 0
  let resIndex = 0
  // 新建数组result，长度为分割后块的数量
  const result = new Array(Math.ceil(length / size))
  // 切割 array 放于 result 中，注意 index，每次切割，index增加size长度
  while (index < length) {
    result[resIndex++] = slice(array, index, (index += size))
  }
  return result
}
```

#### 备注
`Math.round()`  “四舍五入”， 该函数返回的是一个四舍五入后的的整数  
`Math.ceil()`  “向上取整”， 即小数部分直接舍去，并向正数部分进1  
`Math.floor()`  “向下取整” ，即小数部分直接舍去