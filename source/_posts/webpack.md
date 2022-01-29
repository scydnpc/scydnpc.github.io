---
title: webpack
cover: false
categories:
- 前端
tags:
- webpack
---
## webpack

### 概念
![Image text](/images/webpack_01.jpg)
<center>核心概念脑图</center>
****
#### 入口   `entry`
默认：`./src`

单个入口用法：`entry: string|Array<string>`
```js
const config = {
  entry: './path/to/my/entry/file.js'
};

// entry 属性的单个入口语法，是下面的简写：

const config = {
  entry: {
    main: './path/to/my/entry/file.js'
  }
};
```

对象语法：`entry: {[entryChunkName: string]: string|Array<string>}`
```js
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};
```
#### 输出  `output`
默认：`./dist`
用法：
```js
// 在 webpack 中配置 output 属性的最低要求是，将它的值设置为一个对象，包含 filename 和 path
const config = {
  output: {
    filename: 'bundle.js',
    path: '/home/proj/public/assets'
  }
};

module.exports = config;
```
**多个起点的情况（使用占位符确保文件具有唯一名称）**
```js
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}
// 写入到硬盘：./dist/app.js, ./dist/search.js
```
#### 模式  `mode`
描述：提供 `mode` 配置选项，告知 `webpack` 使用相应模式的内置优化。
分 `development` 和 `production`
#### loader
- 用于处理非 JavaScript 文件，webpack 自身只理解 JavaScript
- 在 webpack 配置中定义 loader 时，要定义在 module.rules 中

🌰：你可以使用 `loader` 告诉 `webpack` 加载 `CSS` 文件，或者将 `TypeScript` 转为 `JavaScript`。为此，首先安装相对应的 `loader`：
```shell
npm install --save-dev css-loader
npm install --save-dev ts-loader
```

三种方式使用loader
**配置  `Configuration`**
```js
module: {
  rules: [
    {
      test: /\.css$/,
      use: [
        { loader: 'style-loader' },
        {
          loader: 'css-loader',
          options: {
            modules: true
          }
        }
      ]
    }
  ]
}
```
**内联**
可以在 import 语句或任何等效于 "import" 的方式中指定 loader。使用 ! 将资源中的 loader 分开。分开的每个部分都相对于当前目录解析。
```js
import Styles from 'style-loader!css-loader?modules!./styles.css';
```
**CLI**
```js
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```
这会对 .jade 文件使用 jade-loader，对 .css 文件使用 style-loader 和 css-loader。
#### 插件  `plugins`