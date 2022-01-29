---
title: Js防抖与节流
date: 2021-12-06
categories:
- 前端
tags:
- javascript
- 防抖节流
---
## 概念
### 函数防抖（debounce）
```
在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。
```
实际🌰：监听input输入来请求接口，未进行防抖时，键入内容就会请求
```javascript
//模拟一段ajax请求
function ajax(content) {
  console.log('ajax request ' + content)
}

let inputa = document.getElementById('unDebounce')

inputa.addEventListener('keyup', function (e) {
    ajax(e.target.value)
})
```
优化：
```javascript
//模拟一段ajax请求
function ajax(content) {
  console.log('ajax request ' + content)
}

function debounce(fun, delay) {
    return function (args) {
        clearTimeout(fun.id)
        fun.id = setTimeout( () => {
            fun.call(this, args)
        }, delay) 
        // setTimeout 返回一个唯一id，在delay时间内clearTimeout 该id可阻止事件
        // 该处的作用是：事件触发时间内再次触发事件则停止原事件，重新计时
    }
}
    
let inputb = document.getElementById('debounce')

let debounceAjax = debounce(ajax, 500)

inputb.addEventListener('keyup', function (e) {
        debounceAjax(e.target.value)
    })
```
**函数防抖就像法师发技能的时候要读条，技能读条没完再按技能就会重新读条。**

### 函数节流（throttle）
```
规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。
```

实际🌰：需求：无论多快输入，但每1s执行一次请求。
```javascirpt
function throttle(fun, delay) {
    let last, deferTimer
    return function (args) {
        let that = this
        let _args = arguments
        let now = +new Date()
        clearTimeout(deferTimer) // 重复调用清空上一次的setTime
        // 当第一次调用 或者 键入操作持续一秒后（此时由于clear的作用，last一直没更新，last + delay 会小于now） 会走 else分支
        if (last && now < last + delay) {
            deferTimer = setTimeout(function () {
                last = now
                fun.apply(that, _args)
            }, delay)
        }else {
            last = now
            fun.apply(that,_args)
        }
    }
}

let throttleAjax = throttle(ajax, 1000)

let inputc = document.getElementById('throttle')
inputc.addEventListener('keyup', function(e) {
    throttleAjax(e.target.value)
})
```

**函数节流就是fps游戏的射速，就算一直按着鼠标射击，也只会在规定射速内射出子弹。**

## 总结
- 函数防抖和函数节流都是防止某一时间频繁触发，但是这两兄弟之间的原理却不一样。
- 函数防抖是某一段时间内只执行一次，而函数节流是间隔时间执行。

## 实际应用
- debounce
  - search搜索联想，用户在不断输入值时，用防抖来节约请求资源。
  - window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次
- throttle
  - 鼠标不断点击触发，mousedown(单位时间内只触发一次)
  - 监听滚动事件，比如是否滑到底部自动加载更多，用throttle来判断

## lodash 中的防抖节流方法

```
// 安装 `lodash`
npm i --save lodash
// 引用
import _ from 'lodash'

// 使用防抖
const debounced = _.debounced(func, wait, options)
```
**各参数**
`func` (Function): 要防抖动的函数。    
`[wait=0]` (number): 需要延迟的毫秒数。  
`[options=]` (Object): 选项对象。  
`[options.leading=false]` (boolean): 指定在延迟开始前调用。  
`[options.maxWait]` (number): 设置 func 允许被延迟的最大值。  
`[options.trailing=true]` (boolean): 指定在延迟结束后调用。

**需要注意的点**  
`_.debounced()`的返回值为`func`的返回值，默认参数情况（例如：`_.debounced(func, 500)`）在防抖阶段，因为还未执行`func`的`return`，返回为`undefined`  

**节流`_.throttle()`**
  
## 参考
作者：薄荷前端  
链接：https://juejin.cn/post/6844903669389885453  
来源：稀土掘金
