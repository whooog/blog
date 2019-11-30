---
title: vuex
date: 2019-11-30 11:42:20
tags:
categories:
   - vue
---

**vuex 核心概念**


 * store : 类似容器，包含应用的大部分状态，一个页面只能有一个store,状态存储是响应式的，不能直接改变store中的状态，唯一的途径是显示提交mutations
 
 * state : 用来存放数据
 
 * getters : 和组件的 computed 类似，方便直接生成一些可以直接用的数据，当组装的数据要在多个页面使用时，就可以使用 getters 来做

 * mutations : 唯一修改状态的事件回调函数
 
 * actions : 包含异步操作，提交mutations改变状态
 
 * modules : 将 store 分割成不同的模块
