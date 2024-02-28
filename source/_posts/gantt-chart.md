---
title: 原生JS造轮子--甘特图
date: 2022-04-09 17:25:41
updated: 2022-04-09 17:25:41
categories:

- 功能组件
  tags:
- 造轮子

---

目标是构建一个重用的Web组件`gantt-chart`.

## 需求分析

基本功能:

- 在两种视图之间进行选择：年/月或月/日。
- 过滤开始时间和结束时间
- 该图表呈现可以通过拖放移动的给定作业列表。更改反映在对象的状态中。
- 在下面，您可以在两个视图中看到生成的甘特图。在每月版本中，我以三个工作为例。

## 组件设计

### 使用方法

```html

<gantt-chart id="g1"/>

```

```javascript

import GanttChart from "./gantt-chart.js";

const chart = document.querySelector("#g1");

const jobs = [
  {id: "j1", start: new Date("2021/6/1"), end: new Date("2021/6/4"), resource: 1},
  {id: "j2", start: new Date("2021/6/4"), end: new Date("2021/6/13"), resource: 2},
  {id: "j3", start: new Date("2021/6/13"), end: new Date("2021/6/21"), resource: 3},
];
const p_jobs = [];

chart.resources = [{id:1, name: "Task 1"}, {id:2, name: "Task 2"}, {id:3, name: "Task 3"}, {id:4, name: "Task 4"}];

jobs.forEach(job => {

  const validator = {
    set: function(obj, prop, value) {

      console.log("Job " + obj.id + ": " + prop + " was changed to " + value);
      console.log();
      obj[prop] = value;
      return true;
    },

    get: function(obj, prop){
      return obj[prop];
    }
  };

  const p_job = new Proxy(job, validator);
  p_jobs.push(p_job);
});

chart.jobs = p_jobs;


```






