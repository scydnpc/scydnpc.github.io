---
title: 事件循环Eventloop
date: 2021-09-07
categories:
- 前端
tags:
- javascript
---
### 1. javascript是单线程语言
### 2. 所有的线程，都是有同步队列，和异步队列
立即执行，如函数 是同步；promise，ajax为异步
### 3. 任务队列-事件循环
同步任务会立刻执行，进入到主线程当中，异步任务会被放到任务队列（Event Queue）当中。  
等待同步代码执行完毕后，返回来，再将异步中的任务放到主线程中执行,反复这样的循环，这就是事件循环。也就是先执行同步，返回来按照异步的顺序再次执行
### 4. 宏观任务和微观任务（先执行微观任务，再执行宏观任务）
宏观任务主要包含：setTimeout、setInterval、script(整体代码)、I/O、UI 交互事件、setImmediate(Node.js 环境)  
微观任务主要包括：Promise、MutaionObserver、process.nextTick(Node.js 环境)