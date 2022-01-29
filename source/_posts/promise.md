---
title: promise
date: 2021-12-03
categories:
- 前端
tags:
- javascript
- promise
---
### 概念
Promise 是异步编程的一种解决方案： 从语法上讲，promise是一个对象，从它可以获取异步操作的消息；从本意上讲，它是承诺，承诺它过一段时间会给你一个结果。

### promise 有哪三种状态：
pending(等待态)、fulfiled(成功态)、rejected(失败态)

### promise 使用来解决什么问题的？
回调地狱，代码难以维护， 常常第一个的函数的输出是第二个函数的输入这种现象
promise可以支持多个并发的请求，获取并发请求中的数据
这个promise可以解决异步的问题，本身不能说promise是异步的

### promise是一个构造函数，自己身上有all、reject、resolve这几个眼熟的方法，原型上有then、catch等同样很眼熟的方法。
promise的构造函数接收一个参数：函数，并且这个函数需要传入两个参数：  
- resolve ：异步操作执行成功后的回调函数
- reject：异步操作执行失败后的回调函数  

```javascript
let p = new Promise((resolve, reject) => {
    //做一些异步操作
    setTimeout(() => {
        console.log('执行完成');
        resolve('我是成功！！');
    }, 2000);
});
```

### then
then中可以传递两个参数--请求成功（resolved）的函数和请求失败（reject）的函数
```javascript
let p = new Promise((resolve, reject) => {
    //做一些异步操作
  setTimeout(function(){
        var num = Math.ceil(Math.random()*10); //生成1-10的随机数
        if(num<=5){
            resolve(num);
        }
        else{
            reject('数字太大了');
        }
  }, 2000);
});
p.then((data) => {
        console.log('resolved',data); // resolved 5
    },(err) => {
        console.log('rejected',err); // rejected 数字太大了
    }
);
```

### catch的用法
其实它和then的第二个参数一样，用来指定reject的回调。用法是这样：
```javascript
p.then((data) => {
    console.log('resolved',data);
}).catch((err) => {
    console.log('rejected',err);
});
// 效果和写在then第二个参数里面一样
catch会把then中的报错展示出来，例如:
p.then((data) => {
    console.log('resolved',data);
    console.log(somedata); //此处的somedata未定义
})
.catch((err) => {
    console.log('rejected',err);
});
```
控制台输出 ： rejected ReferenceError：somedata is not defined at p.then
与try/catch有相同的功能

### Promise.all
all 方法提供了并行执行异步操作的能力，并且在所有异步操作执行完后才执行回调，即all中promise 的 resolve 或者 reject执行完了再执行then  
例：
```javascript
let Promise1 = new Promise(function(resolve, reject){
    setTimeout(()=>{
        resolve(console.log(1))
    },1000)
})
let Promise2 = new Promise(function(resolve, reject){resolve(console.log(2))})
let Promise3 = new Promise(function(resolve, reject){resolve(console.log(3))})
let p = Promise.all([Promise1, Promise2, Promise3])
p.then(function(){
    // 三个都成功则成功
    console.log('aaaaaa')  // 如果promise123 不通过resolve输出，这段话不执行
}, function(){
    // 只要有失败，则失败 
})
```
all的作用：
有了all，你就可以并行执行多个异步操作，并且在一个回调中处理所有的返回数据，是不是很酷？有一个场景是很适合用这个的，一些游戏类的素材比较多的应用，打开网页时，预先加载需要用到的各种资源如图片、flash以及各种静态文件。所有的都加载完后，我们再进行页面的初始化。  

参考： 作者：蔓蔓雒轩  链接：https://juejin.cn/post/6844903607968481287。