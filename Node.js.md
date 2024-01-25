# 初识Node.js

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。

浏览器是JavaScript的前端运行环境

nodejs是JavaScript的后端运行环境

# 模块加载机制

## 优先从缓存中加载

模块第一次被加载后会被缓存.意味着即使用require()多次调用模块,模块中的代码也只会在第一次时被执行

## 内置模块的加载机制

内置模块加载优先度最高,如果第三方模块与内置模块重名,nodejs会优先加载内置模块

例:

```js
// 加载的永远是内置模块fs
const fs = require('fs')
```

## 自定义模块的加载机制

使用require()加载自定义模块时,必须要加上路径标识符"./"或"../"，否则会被当做内置模块或第三方模块加载

同时，在使用require()加载自定义模块时，nodejs会按如下顺序进行加载

1.按照确切的文件名进行加载

2.补全.js扩展名进行加载

3.补全.json扩展名进行加载

4.补全.node扩展名进行加载

5.加载失败,终端报错

## 第三方模块的加载机制

![image-20221012091733151](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20221012091733151.png)

# Express框架

## 概念:基于 [Node.js](https://nodejs.org/en/) 平台，快速、开放、极简的 Web 开发框架

## 通俗理解:Express框架的作用和nodejs内置模块作用类似,是用来快速创建web服务器

## 本质:npm上的第三方包

