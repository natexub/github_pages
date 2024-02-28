---
title: 读《代码整洁之道》
date: 2019-03-29 17:32:45
updated: 2019-03-29 17:32:45
categories:
tags:
- 读书
---

注：**编号**按照原作方便索引，**引用**为原文或有把握的原文概况，**正文**为批注和想法。

## 2. 命名

### 2.2 名副其实

> 一旦发现好的名称，就替换掉旧的

借助现代IDE，重命名变得更容易，Jetbrains系列一个`shift+f6`就能重构-重命名，这个功能就略胜VSCode一筹了（截至目前）。

> 如果名称需要用注释来补充，那就不算名副其实

英语的话是这样。但是汉语环境下，对写代码的人来说英文容易不达意，对读代码的人来说并不直观。尽量做到变量名容易理解的同时，对于复杂不直观的命名，还是加上注释，降低大家的理解成本。

手抄一遍案例实践以加深印象：

``` java
public List<int[]> getThem() {
	List<int[]> list1 = new ArrayList<int[]>()
	for (int[] x: the list)
		if(x[0] === 4)
			list1.add(x)
	return list1;
}
```

优化为:

```java
public List<int[]> getFlaggedCells(){
	List<int[]> flaggedCells = new ArrayList<int[]>()
	for (int[] cell: gameBord)
		if(cell[STATUE_VALUE] === FLAGGED) // 或者cell.isFlagged()
			flagCells.add(cell)
	return flagCells
}
```

### 2.3 避免误导

> 避免使用会造成本意相悖的专有名称。比如平台专有名词，数据类型名称：只有确实为`List`类型时，才使用`acountList`，否则`acounts`等更好


> 避免相似的其差异却难以察觉的命名。

IDE可以自动补全，命名时还要考虑与IDE补全系统的配合。

### 2.4 做有意义的区分

有了`customer`做变量, 程序员如果还需要引入一个相关的变量，为了满足不重名的需要自然要做区分，比如`customer` 。这时会引入一个让读者疑惑的点，如何区分这两者区别，这种情况需要做明确约定。

### 2.5