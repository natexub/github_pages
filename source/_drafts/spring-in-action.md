---
title: Spring 实战 读书笔记
date: 2023-02-16 14:35:20
updated: 2023-02-16 14:35:20
categories:
- Java
tags:
- Java
- 读书笔记
---

很久之前读的第四版，如今出到第六版，已经是从SpringBoot开始讲了，xml配置bean的方式被视为过时。今天想重新梳理一下Spring。



### 概念

##### POJO：

POJO代表“Plain Old Java Object”（简单老式Java对象），它是指普通的、简单的Java对象。

###### 应用场景：
- 在Java开发中，通常用于表示简单的数据对象，不包含业务逻辑或特定的框架依赖。
- Java的企业级开发中，POJO常被用于作为数据传输对象（DTO）或持久化对象（Entity）等。

###### 举例

```

```

##### Bean

一个普通的Java对象（POJO），当它被Spring管理的时候，作为管理对象，称之为Bean。

Spring通过依赖注入装配POJO，帮助这些对象保持松耦合。为Spring配置Bean的方法有：
- Java
- XML
- SpringBoot自动化配置

##### SpEL

在第四版老书中，对xml方式配置Bean进行了举例，向Bean的构造函数传参时，用到了Spring表达式。[Spring Expression Language (SpEL) :: Spring Framework](https://docs.spring.io/spring-framework/reference/core/expressions.html)

