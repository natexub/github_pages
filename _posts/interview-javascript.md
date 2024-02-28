---
title: 面试准备-Javascript基础
date: 2020-08-22 17:55:08
updated: 2020-08-22 17:55:08
categories:
 - interview
 - javascript
tags:
---


作用域的判断

```js
var timer = function(name) {
    var start = new Date();
    return {
        stop: function() {
            var end  = new Date();
            var time = end.getTime() - start.getTime();
            console.log('Timer:', name, 'finished in', time, 'ms');
        }
    }
};
```