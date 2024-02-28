---
title: Axios
date: 2021-07-19 15:52:10
updated: 2021-07-19 15:52:10
categories:
tags:
---

## 摘要

首先通过阅读[源码](https://github.com/axios/axios/tree/v1.0.0-alpha.1)，讲清楚Axios Interceptor的原理。(最新检验更新根据v1.0.0-alpha.1)

在Axios[文档](https://axios-http.com/zh/docs/intro)的特性中，列出了Axios的特性，文中讲清楚这些特性的实现。

* 从浏览器创建 XMLHttpRequests
* 从 node.js 创建 http 请求
* 支持 Promise API
* 拦截请求和响应
* 转换请求和响应数据
* 取消请求
* 自动转换JSON数据
* 客户端支持防御XSRF

## Axios Interceptor

### Interceptor pattern

第一次接触拦截器概念是在学习Spring MVC时, 再次遇到它让我意识到拦截器是一种常用的思想/软件模式. 因此在此特地总结一下.

在软件模式分类中, 拦截器模式归属于建筑模式, 见[Interceptor](http://software-pattern.org/Interceptor)




### 使用

```javascript

// request 拦截
axios.interceptor.request.use(
  function (config){
    // 在发送请求前要做些什么
    return config;
  }, function (error){
    // 对请求错误做些什么
  }
);

// response拦截
axios.interceptor.response.use(
  function (response) {
    // 2xx 范围内状态码都会除非该函数
    // 对响应数据做些什么
  },
  function (error) {
    // 任何超出2xx范围之外的状态码都会触发该函数
    // 对响应错误做些什么
    return Promise.reject(error)
  }
);

```
## 应用场景

### 统计请求耗时

### 对后端状态码进行拦截



## axios怎么区分客户端和服务端的请求
在Axios文档的特性中, 首先指出它是isomorphic(同构), 即同一套代码既可以通过http模块应用在server-side的Node.js中, 也可以 通过XMLHttpRequests应用于client-side的浏览器中.

## 跟fetch, jQuery.ajax的对比

## Axios封装

## 实际开发中的替代品

## 相关问题

React 18 中实验模式 多次请求

## 参考