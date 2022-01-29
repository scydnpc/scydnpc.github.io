---
title: (转载) lodash源码 eq
date: 2021-12-27
categories:
- 前端
tags:
- javascript
- lodash
---
### _.eq(value, other)
> 介绍：执行 `SameValueZero` 比较两者的值，来确定它们是否相等。
#### 🌰
```javascript
var object = { 'a': 1 };
var other = { 'a': 1 };
 
_.eq(object, object);
// => true
 
_.eq(object, other);
// => false
 
_.eq('a', 'a');
// => true
 
_.eq('a', Object('a'));
// => false
 
_.eq(NaN, NaN);
// => true
```
#### 规则比较
> 前排提示：以下提到的规则，`SameValueNonNumber` 是基本，`Strict Equality Comparison` 、`SameValue` 和 `SameValueZero` 只是在对待 `+0`、`-0` 和 `NaN` 上有区别。
##### 基础规则：SameValueNonNumber

这个规范规定比较的值 x 和 y 都不为 Number 类型，照抄规范如下：
1. `x` 的类型不为 `Number` 类型
1. `y` 的类型与 `x` 的类型一致
1. 如果 `x` 的类型为 `Undefined` ，返回 `true`
1. 如果 `x` 的类型为 `Null` ，返回 `true`
1. 如果 `x` 的类型为 `String`，并且 `x` 和 `y` 的长度及编码相同，返回 `true`，否则返回 `false`
1. 如果 `x` 的类型为 `Boolean` ，并且 `x` 和 `y` 同为 `true` 或同为 `false` ，返回 `true`，否则返回 `false`
1. 如果 `x`的类型为 `Symbol` ，并且 `x` 和 `y` 具有相同的 `Symbol` 值，返回 `true`，否则返回 `false`
1. 如果 `x` 和 `y` 指向同一个对象，返回 `true`， 否则返回 `false`
##### Strict Equality Comparison
js 中的全等（`===`）便是遵循这个规范，照搬规范如下：
1. 如果 `x` 和 `y` 的类型不同，返回 `false`
1. 如果 `x` 的为 `Number` 类型：
    - a. 如果 `x` 为 `NaN` ，返回 `false`
    - b. 如果 `y` 为 `NaN` ，返回 `false`
    - c. 如果 `x` 和 `y` 的数值一致，返回 `true`
    - d. 如果 `x` 为 `+0` 并且 `y` 为 `-0` ，返回 `true`
    - e. 如果 `x` 为 `-0` 并且 `y` 为 `+0` ，返回 `true`
    - f. 返回 `false`
1. 按照 `SameValueNonNumber` 的结果返回
##### SameValue
1. 如果 `x` 和 `y` 的类型不同，返回 `false`
1. 如果 `x` 的为 `Number` 类型：
    - a. 如果 `x` 为 `NaN` 并且 `y` 为 `NaN` ，返回 `true`
    - b. 如果 `x` 为 `+0` 并且 `y` 为 `-0` ，返回 `false`
    - c. 如果 `x` 为 `-0` 并且 `y` 为 `+0` ，返回 `false`
    - d. 如果 `x` 和 `y` 的数值一致，返回 `true`
    - e. 返回 `false`
1. 按照 `SameValueNonNumber` 的结果返回
##### SameValueZero
这个是 `_.eq` 遵循的规范，如下：
1. 如果 `x` 和 `y` 的类型不同，返回 `false`
1. 如果 `x` 的为 `Number` 类型：
    - a. 如果 `x` 为 `NaN` 并且 `y` 为 `NaN` ，返回 `true`
    - b. 如果 `x` 为 `+0` 并且 `y` 为 `-0` ，返回 `true`
    - c. 如果 `x` 为 `-0` 并且 `y` 为 `+0` ，返回 `true`
    - d. 如果 `x` 和 `y` 的数值一致，返回 `true`
    - e. 返回 `false`
1. 按照 `SameValueNonNumber` 的结果返回

#### 源码
```javascript
function eq(value, other) {
  return value === other || (value !== value && other !== other)
}
```
`Strict Equality Comparison` 和 `SameValueZero` 只在对待 `NaN` 上有区别。
所以在此基础上使用`value !== value && other !== other` 修改 `NaN` 的处理即可

#### 可以用 Object.is() 替代吗？
**不能**，`Object.is` 同样是比较两个值是否一样，遵循是的 `SameValue` 规范，`Object.is(+0, -0)` 返回的是 `false`， 所以不能

可以用来判断 `NaN`, `Object.is(NaN, NaN)` 返回的是 `true` ，所以 `eq` 同样可以改成：
```javascript
function eq(value, other) {
  return value === other || Object.is(value, other)
}
```

#### 可以用 isNaN() 吗？
**不能**，如果传入的参数不为 Number 类型，会尝试转换成 Number 类型之后再做是否为 NaN 的判断。所以类似 isNaN('notNaN') 返回的也是 true ，因为字符串 notNaN 会先被转换成 NaN 再做判断，这不是我们想要的结果。

#### 可以用 Number.isNaN() 吗？
为了修复 `isNaN` 的缺陷，`es6` 在 `Number` 对象上扩展了 `isNaN` 方法，只有是 `NaN` 时才会返回 `true`，因此用 Number.isNaN 来判断是安全的。所以 `eq` 同样可以改成以下形式：
```javascript
function eq(value, other) {
  return value === other || (Number.isNaN(value) && Number.isNaN(other))
}
```