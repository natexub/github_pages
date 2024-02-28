---
title: JavaScript异步编程
date: 2021-06-06 01:40:04
updated: 2021-06-06 01:40:04
categories:
- Javascript
tags:
- 异步
---

## 概念

并发与并行区别参见文章{% post_link concurrency-and-parallelism '并行和并发' %}.

### 异步和同步

在两种场景: 网络通信和计算机编程中的定义稍微存在差别

#### 通信/电信

在网络通信中, *异步通信(asynchronous communication)*指的是发送方和接收方两端并不依靠一个精确同步的时钟进行实时的同步的传输. 比如在应用层的电子邮件和离线消息, 而电话视频等实时流媒体则是*同步通信*. 既然不是同步的传输, 那么需要解决通信的开始结束的通知或者回调的问题.

#### 计算机编程

在计算机编程中, *异步编程*指的是主程序发起执行一段代码, 不会立刻返回一个执行这段代码的结果, 而是会在将来通过某种方式来返回执行结果的编程模式.



## JavaScript并发模型





## JavaScript 异步编程方式

## Promise

```ts
interface PromiseConstructor {
	new <T>(executor: (resolve: (value: T | PromiseLike<T>) => void, reject: (reason?: any) => void) => void): Promise<T>;
	then<TResult1 = T, TResult2 = never>(onfulfilled?: ((value: T) => TResult1 | PromiseLike<TResult1>) | undefined | null, onrejected?: ((reason: any) => TResult2 | PromiseLike<TResult2>) | undefined | null): Promise<TResult1 | TResult2>;
}
```

### 样例

以连续三个请求为例，后两个请求依赖于前一个的结果。

```js
console.log(`初始值2, 开始执行操作...`)
发送一个请求(2)  
  .then((result) => {  
    console.log(`结果接收: ${result}, 开始执行下一请求`)  
    return fetch(result)  
      .then((result) => {  
          console.log(`结果接收: ${result}, 开始执行下一请求`)  
          return fetch(result)  
            .catch((e) => {  
              console.log(`2出现错误:${e.message}`)  
            })  
        },  
      )  
  })  
  .then((res) => {  
    console.log(`最终结果: ${res}`)  
  })  
  .catch((e) => {  
    console.log(`出现错误! ${e.message}`)  
  })

function 发送一个请求(n) {  
  return fetch(n)  
}
function errorMayOccur() {
  const randomNumber = Math.random() * 10
  if (randomNumber > 9) {
    throw new Error('网络错误')
  }
}

function fetch(option) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      try {
        errorMayOccur()
        resolve(option + 1)
      } catch (e) {
        reject(e)
      }
    }, 2000)
  })
}
```

大致对应的`await`/`async`形式：

```js
  try {
    console.log(`初始值2, 开始执行操作...`)
    const result1 = await 发送第一个请求(2)
    console.log(`结果接收: ${result1}, 开始执行下一请求`)
    const result2 = await fetch(result1)
    console.log(`结果接收: ${result2}, 开始执行下一请求`)
    const result = await fetch(result)
    console.log(`最终结果: ${result}`)
  } catch (e) {
    console.log(`出现错误!${e.message}`)
  }
```

### 容易犯的问题

#### 没有按照本意形成链式的调用

如果then没有返回值，`then`返回的Promise得到的参数executor中的reslove得到的参数会是`undefined`，即：
```js
doSomething()
  .then((url) => {
    // 没有返回
    fetch(url);
  })
  .then((result) => {
    // result 为 undefined，无法得到上一个then中的结果，同时没有形成链式调用：这里面的内容和上一个then中的代码会同步执行而不是异步。
  });
```
同时没有形成链式调用：后面`then`的代码和上一个then中的代码会形成竞态同步执行而不是异步。

这一错误在`await`/`async`形式下更容易犯，要意识到`await`是`Promise`和`Generator`组合成的语法糖，

### 嵌套

```js
doSomething()
  .then((url) =>
    fetch(url)
      .then((res) => res.json())
      .then((data) => {
        listOfIngredients.push(data);
      })
      .catch((e) => {})
  )
  .then(() => {
    console.log(listOfIngredients);
  });
```

嵌套容易造成增加导致理解上的成本，产生粗心错误。嵌套的一个作用是用来缩小错误捕获的范围，在内部链结尾的catch总比一长条的位置更精确。

### Promise.resolve()



#### 应用场景

### Promise.all()

#### 应用场景

##### 在`Array.map()`方法中使用

样例：打开多个文件，返回描述符。

```js
function openFiles(...filepaths) {  
  if (filepaths.length === 1) {  
    const filePath = filepaths[0]  
    return fs.open(filePath)  
  } else if (filepaths.length > 1) {  
    return Promise.all(  
      filepaths.map(  
        (filePath) => fs.open(filePath),  
      ),  
    )  
  }  
}
```

## `await`/`async`

`await`是`Promise`和`Generator`组合成的语法糖，

```js
async function foo() {
  await 1;
}
```

效果类似于：

```js
function foo() {
  return Promise.resolve(1).then(() => undefined);
}
```


