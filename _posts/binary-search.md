---
title: 二分查找，基础但不简单
date: 2022-02-16 14:35:20
updated: 2022-02-16 14:35:20
categories:
tags:
---

有序数组的二分查找很基础，但有一些细节不可以忽略。掌握这些细节后，对二分查找有了更深刻的理解。

### 区间开闭

在思考阶段，需要确定区间的开闭原则，根据确定好的原则写出代码，继而确定代码中的边界条件。

#### 左右皆闭

若选择左右皆闭，则`leftPos`与`rightPos`相等是被允许的，且左右边界都会被用于查找。 

因此，循环条件确定为`leftPos <= rightPos`。

更新`leftPos`与`rightPos`时已经确定`midPos`处的值并非目标值，又因为`leftPos`与`rightPos`都会被用于查找，因此，更新`leftPos`与`rightPos`时，将已经确定的非目标值排除在外。

```c++
if (target < sortedNumbers[midPos]) {  
    rightPos = midPos - 1; // 不采用midPos，因为左右区间皆闭，右边界在检索范围内，而mid处已确定非目标，故将右边界更新为midPos - 1  
} else if (sortedNumbers[midPos] < target) {  
    leftPos = midPos + 1;  
}
```

同理，右边界的初始值设置为：

```c++
int rightPos = (int) sortedNumbers.size() - 1;
```


#### 左闭右开

若选择左闭右开，`leftPos`与`rightPos`相等无意义。右边界不会被用于查找。

因此循环条件确定为：`leftPos < rightPos`。

更新`rightPos`时，由于右边界不会被用于查找，因此将右边界更新为`midPos`处的值。

同理，右边界的初始值设置为：

```c++
int rightPos = (int) sortedNumbers.size();
```

### 均值计算避免相加数值溢出

为避免计算两int型数均值时数值溢出，采用这种方法：
```c++
int midPos = leftPos + ((rightPos - leftPos) / 2);
```