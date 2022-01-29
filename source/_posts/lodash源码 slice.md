---
title: (转载) lodash源码 slice
date: 2021-12-15
categories:
- 前端
tags:
- javascript
- lodash
---
##  _.slice(array, [start=0], [end=array.length])
> **介绍:**   裁剪数组`array`，从`start` 位置开始到`end`结束，但不包括`end` 本身的位置。**这个方法用于代替Array#slice 来确保数组正确返回**  

**为什么需要替代:**  
lodash 的 slice 会将数组当成密集数组对待，原生的 slice 会将数组当成稀疏数组对待。

#### 密集数组VS稀疏数组

> 稀疏数组就是包含从0开始的不连续索引的数组。通常，数组的length属性值代表数组中元素的个数。如果数组是稀疏的，length属性值大于元素的个数。

如果数组是稀疏的，那么这个数组中至少有一个以上的位置不存在元素（undefined算元素）。  
🌰
```javascript
// sparse 的 length 为10，但是 sparse 数组中没有元素，是稀疏数组
var sparse = new Array(10) 
// dense 每个位置都是有元素的，虽然每个元素都为undefined，为密集数组
var dense = new Array(10).fill(undefined)
```
#### 源码总览
```javascript
function slice(array, start, end) {
  // array == null, 非 === ,包含undefined的判断 undefined == null true
  let length = array == null ? 0 : array.length
  // 不传参的情况
  if (!length) {
    return []
  }
  // 此部分判断是否带start或end的参
  // 不带的时候赋予初始值
  start = start == null ? 0 : start
  end = end === undefined ? length : end
  // start 为负数的情况
  if (start < 0) {
    // 如果负数取反后比数组的长度还要大，即超出了数组的范围，则取值为0
    start = -start > length ? 0 : (length + start)
  }
  // end最大取数组长度
  end = end > length ? length : end
  // end 小于0
  if (end < 0) {
    end += length
  }
  // 新数组的长度
  length = start > end ? 0 : ((end - start) >>> 0)
  start >>>= 0

  let index = -1
  const result = new Array(length)
  // 截取并返回新数组
  while (++index < length) {
    result[index] = array[index + start]
  }
  return result
}
```
**start**  
> - 如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取。
> - 如果省略，则从索引0开始

**end**
> - 如果该参数为负数，则它表示在原数组中的倒数第几个元素结束制取。
> - 如果end被省略，则slice会一直提取到原数组的末尾。
> - 如果end大于数组长度，slice也会一直提取到原数组末尾。

**关于 `start >>>= 0`**  
- 使用 `>>>` 来确保 `start` 参数为整数或0。  
- 因为 `lodash` 的 `slice` 除了可以处理数组外，也可以处理类数组( `arguments` 对象和 `DOM` 方法的返回结果)，因此第一个参数 `array` 可能为一个对象， `length` 属性不一定为数字。
- 更多知识请参考“移位运算符”