---
title: (转载) lodash源码 compact
date: 2021-12-20
categories:
- 前端
tags:
- javascript
- lodash
---
## _.compact(array)
> 介绍：创建一个新数组，包含原数组中所有的非假值元素。例如`false` , `null` , `0` , `""` , `undefined` , 和 `NaN` 都是被认为是“假值”。  
🌰
```javascript
var arr = [1,false,2,null,3,0,4,NaN,5,undefined]
_.compact(arr) // 返回 [1，2，3，4，5]
```
#### 源码
```javascript
function compact(array) {
  let resIndex = 0
  const result = []

  if (array == null) {
    return result
  }

  for (const value of array) {
    if (value) {
      result[resIndex++] = value
    }
  }
  return result
}
```
#### 为什么使用 `for  of`
> 遍历除了 `for  of` 还有 `for`、`for in`，使用 `for`的话，不够间接，下面讨论为什么不使用`for in`

##### for in

for in是以**任意顺序**遍历一个对象的[可枚举属性](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)。  

在数组中，数组的索引是可枚举属性，可以用 for...in 来遍历数组的索引，数组中的稀疏部分不存在索引，可以避免用 for 循环造成无效遍历的弊端。

但是，for...in 的遍历顺序依赖于执行环境，不同执行环境的实现方式可能会不一样。单凭这一点，就断然不能在数组遍历中使用 for...in，大多数情况下，顺序对于数组的遍历都相当重要。

另外，先看个🌰
```javascript
var arr = [1,2,3]
arr.foo = 'foo'
for (let index in arr) {
  console.log(index)
}
```
在这个例子中，你期望输出的是 0,1,2，但是最后输出的可能是 0,1,2,foo （for...in 不能保证顺序）。因为 foo 也是可枚举属性，在 for..in 会被遍历出来。

##### for of
`for...of` 循环内部调用的就是数组原型链上的 `Symbol.iterator` 方法。

`Symbol.iterator` 在调用的时候会返回一个遍历器对象，这个遍历器对象中包含 `next` 方法，`for...of` 在每次循环的时候都会调用 `next` 方法来获取值，直到 `next` 返回的对象中的 `done`属性值为 `true` 时停止。

手动调用来模拟遍历的过程：
```javascript
const arr = [1,2,3]
const iterator = a[Symbol.iterator]()
iterator.next() // {value: 1, done: false}
iterator.next() // {value: 2, done: false}
iterator.next() // {value: 3, done: false}
iterator.next() // {value: undefined, done: true}
```

因此在不改写 `Symbol.iterator` 的情况下, 使用 `for...of` 来遍历数组是安全的，因为这个方法(`Symbol.iterator`)是数组的原生方法。