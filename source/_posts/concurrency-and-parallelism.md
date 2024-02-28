---
title: 并行和并发
date: 2021-06-05 19:57:42
updated: 2021-06-05 19:57:42
categories:
 - 计算机科学
tags:
 - 操作系统
 - 计算机组成原理
 - 并行
 - 并发
---

网上的解释大多来源于知乎的一篇回答，与我而言，用我下文的这种描述方式，能够更好地把知识串联起来理解。




*并发*(concurrency)是一个**宽泛**的概念, 是指一个系统里同时发起执行多个任务。*并行*(parallelism)指的是同时**独立**处理多个任务. 有时候两个概念会让人理不清楚, 

比如: 

在某些情况下, 可以这样理解: **并行**是**并发**的子集. 并发编程有两种类型：**非并行并发编程**(non-parallel concurrent programming)和**并行并发编程**(parallel concurrent programming)（也称为**并行**), 这时候就产生了一个认识差异, 有很多人会把**并发**特指成**非并行并发**来与**并行**的概念区分开.

在某些情况, **并行**又并不意味着**并发**, 比如下文中提到的SIMD.  

在我看来, 造成混乱的原因有

- 两个词字面意思相近
- 关注的抽象层级不同, 两者之间的关系有着微妙的差异

接下来通过不同抽象层级间的举例, 来帮助理清楚它们的关系.

## 举例理解

### 系统和程序级别

"高并发(high-currency)系统", 就是指一个系统有能力短时间内同时处理很高数量级的请求. 但是这个系统的内部可能对任务进行了并行式的处理.

并发系统内部的各个程序之间可能会是独立松耦合的, 它们并行执行, 因此并发系统也会混称为并行系统, 比如map-reduce Hadoop集群这种分布式/并行计算体系, 每个节点都有处理的能力和任务, 共同完成作业. 

### 代码级别

这里是作为应用程序开发者接触最多的抽象层级. 以JavaScript和Go的并发作为例子.

#### JavaScript

JavaScript中的并发模型是**事件队列**(Event loop). 对于**当下**执行的JS代码来说, 向事件队列中安排多个计划在**将来**执行的任务(代码段, 函数), 这些任务就被描述为**并发**执行的. 

具体一下讲

- 使用`setTimeout()`注册定时回调, 
- 使用`addEventListener()`注册事件响应回调,
- 通过`XMLHttpRequest`/`fetch`注册http请求回调

计时的进行, 事件的触发, 请求的进行, 当多个类似这些类型的任务共同发起时, 这些任务就可以被称为**并发**的. 

再具体一点将, 这些任务JavaScript引擎按照脚本代码的指示通过宿主环境提供的接口向宿主环境进行注册, 由宿主环境对这些任务进行**并发**执行, 执行的细节由宿主环境决定, 会存在**并行**执行这些任务的情况. 当宿主事件发现时间到达/事件触发/请求返回就会把相应的回调函数安排到**事件队列**中, 由JavaScript执行引擎去挨个顺序执行. 

如果只关注JavaScript引擎对代码的执行,  不考虑宿主的执行, JavaScript引擎先顺序执行**当下**的代码(注册xxx), 随后如果**事件队列**不为空就顺序执行队列中的任务, 并没有**并行**这种一起执行的情况, 但是我们会说这里有**并发**的存在.

在JavaScript的例子中我们会发现, 站在比代码更高一一点的视野看, 中文有取巧的理解方法 -- **并发**的侧重点在于"**发**"(occur),  发起. 无论是不是同时发生, 至少这些任务是在同一时刻**发起**的. 每个任务之间, 可能会有*竞态(race condition)*的存在, 也可能同时**并行**地执行, 宏观上看是一起发生的, 即**并发**.

#### Golang

与需要宿主环境的脚本语言JavaScript不同, Golang需要与操作系统提供的线程直接进行交互.

Golang中的**并发**例子是goroutine,  不同goroutine中的代码是**并发**执行的, 

###  线程级别

通过线程, 能够实现在一个进程中执行多个控制流,  假如只有一个处理单元时, 这种**并发**是通过分配时间片模拟出来的. 有了多核处理器和超线程的出现, 就允许一个处理器, 通过多线程编程, **并行**地处理多个线程.

```
Concurrency(只有并发没有并行) Concurrency + parallelism(既有并行也有并发)
(Single-Core CPU)           (Multi-Core CPU)
 ___                         ___ ___
|th1|                       |th1|th2|
|   |                       |   |___|
|___|___                    |   |___
    |th2|                   |___|th2|
 ___|___|                    ___|___|
|th1|                       |th1|
|___|___                    |   |___
    |th2|                   |   |th2|

```
代码描述了

以下原文摘自《深入理解计算机系统》

> （8.2.2）......一个逻辑流的执行在时间上与另一个流重叠, 称为并发流(concurrent flow)......并发流的思想与流运行的处理器核数或者计算机数无关. 如果两个流在时间上重叠就是并发的, 基石他们是运行在同一个处理器上, 不过有时候确认**并行流**(parallel flow)是有帮助的, 他是并发流的一个真子集......)

### 指令级别

现代处理器可以同时执行多条指令的属性称为*指令级并行*. 如果处理器可以在一个时钟周期能执行多条指令, 称之为*超标量(superscalar)处理器*. 虽然一个时钟周期内可以执行多条指令, 但是每条指令从开始到结束的执行时间往往需要20个或更多个时钟周期.

### 单指令多数据并行

现代处理器拥有特殊的硬件, 允许一条指令产生多个可以并行执行的操作, 这种方式称为单指令多数据并行. SIMD 主要面向需要对大量数据进行简单、重复计算的图形应用程序和物理计算。

![simd_operation](simd_operation_1.png)

如图所示, SIMD操作是一种向量运算.

但是根据Wikipedia说法

>Such machines exploit [data level parallelism](https://en.wikipedia.org/wiki/Data_parallelism), but not [concurrency](https://en.wikipedia.org/wiki/Concurrent_computing): there are simultaneous (parallel) computations, but each unit performs the exact same instruction at any given moment (just with different data).

>这种机器利用[数据级并行性](https://en.wikipedia.org/wiki/Data_parallelism)，但不利用[并发性](https://en.wikipedia.org/wiki/Concurrent_computing)：存在同时（并行）计算，但每个单元在任何给定时刻执行完全相同的指令（只是使用不同的数据）



## 参考

- [Single instruction, multiple data - Wikipedia](https://en.wikipedia.org/wiki/Single_instruction,_multiple_data)

- [What is the difference between concurrency and parallelism? - Stack Overflow](https://stackoverflow.com/questions/1050222/what-is-the-difference-between-concurrency-and-parallelism/30761061#30761061)
