---
title: yarn和npm命令对比
date: 2022-1-25
---
```js
npm                                     yarn

npm init                                yarn init              // 初始化
npm i | install                         yarn  (install)        // 安装依赖包
npm i x --S | --save                    yarn add  x            // 安装生产依赖并保存包名
npm i x --D | --save-dev                yarn add x -D          // 安装开发依赖并保存包名
npm un | uninstall  x                   yarn remove            // 删除依赖包
npm i -g | npm -g i x                   yarn global add x      // 全局安装
npm un -g x                             yarn global remove x   // 全局下载
npm run dev                             yarn dev | run dev     // 运行命令
```