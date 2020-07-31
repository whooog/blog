---
title: 创建react项目步骤
date: 2019-12-09 10:05:02
tags:

categories:
    - React
---

** 创建react项目步骤 **

1) 使用create-react-app创建一个项目

```
npm install -g create-react-app
create-react-app 项目名
```

2) 打包发布项目

```
npm run build       // 生成build文件夹
npm install -g serve
serve build
```

3) 创建项目目录

![](/images/catalog.png)

4) 引入ui库 antd

```
npm install --save antd-mobile
```

5) 解决移动端click点击延迟3秒的bug

```
npm install fastclick

index.js 引入

import FastClick from 'fastclick'
FastClick.attach(document.body)


```

6) 实现组件的按需打包

```
npm install react-app-rewired customize-cra
 
 创建加载配置的js模块文件：config-overrides.js
 
const { override, fixBabelImports } = require('customize-cra');
 module.exports = override(
   fixBabelImports('import', {
     libraryName: 'antd-mobile',
     libraryDirectory: 'es',
     style: 'css',
   }),
 );

 更改package.json 中的配置
  
  "scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject",
    "client": "serve build"             // 更改build运行命令
  },
```
