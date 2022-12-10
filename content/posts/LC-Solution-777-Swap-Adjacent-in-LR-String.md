---
title: LC-Solution 777. Swap Adjacent in LR String
date: 2022-10-03 21:01:31
tags: [算法]
categories: [Leetcode题解]
---

# [777. 在LR字符串中交换相邻字符](https://leetcode.cn/problems/swap-adjacent-in-lr-string/)

## 题目描述

在一个由 `'L'` , `'R'` 和 `'X'` 三个字符组成的字符串（例如`"RXXLRXRXL"`）中进行移动操作。一次移动操作指用一个`"LX"`替换一个`"XL"`，或者用一个`"XR"`替换一个`"RX"`。现给定起始字符串`start`和结束字符串`end`，请编写代码，当且仅当存在一系列移动操作使得`start`可以转换成`end`时， 返回`True`。

**示例：**

```
输入: start = "RXXLRXRXL", end = "XRLXXRRLX"
输出: True
解释:
我们可以通过以下几步将start转换成end:
RXXLRXRXL ->
XRXLRXRXL ->
XRLXRXRXL ->
XRLXXRRXL ->
XRLXXRRLX
```

**提示：**

- `1 <= len(start) = len(end) <= 10000`。
- `start`和`end`中的字符串仅限于`'L'`, `'R'`和`'X'`。

## 解决方案

### 方法一：模拟

根据题意进行分析模拟，题目将每一个`XL`都替换成了`LX`，将每一个`RX`都替换成了`XR`

通过找规律可发现，替换的过程可以理解为`L`与`R`移动的过程。

- 当`L`的左边为`X`时，`L`可向左移动
  
- 当`R`的右边为`X`时，`R`可向右移动
  
- `L`与`R`无法互相穿过（因为`L`左和`R`右必须为`X`）
  
  - 可得知当`start`与`end`去掉`X`后，剩余字符必须相同，否则返回False

根据此规律，使用双指针`i`与`j`从头到尾遍历`start`与`end`

找到`start`与`end`中`非X`的字符时则停止移动，进行判断

判断条件为：

- 若`start[i] == L and i < j`，又因`L`无法向右移动，直接返回False
  
- 若`start[i] == R and i > j`，又因`R`无法向左移动，直接返回False
  

当双指针遍历字符串结束后，说明中途未返回False，则最终返回True

注意：

`if i != j and (start[i] == 'L') != (i > j)`该行代码摘自[灵山茶艾府](https://leetcode.cn/u/endlesscheng/)大佬的题解，相当于将两个`if`判断条件揉为一个，其本质为：

- `if i < j and start[i] == 'L': return False`
  
- `if i > j and start[i] == 'R': return False`
  

#### Python3

```python
class Solution:
    def canTransform(self, start: str, end: str) -> bool:
        if start.replace('X', '') != end.replace('X', ''): return False
        n = len(start)
        i = j = 0
        while i < n and j < n:
            while i < n and start[i] == 'X': i += 1
            while j < n and end[j] == 'X': j += 1
            if i != j and (start[i] == 'L') != (i > j): return False
            i, j = i + 1, j + 1
        return True
```

#### Java

```java
class Solution {
    public boolean canTransform(String start, String end) {
        if(!start.replace("X", "").equals(end.replace("X", ""))) return false;
        int n = start.length();
        int i = 0, j = 0;
        while(i < n && j < n) {
            while(i < n && start.charAt(i) == 'X') i += 1;
            while(j < n && end.charAt(j) == 'X') j += 1;
            if ((i != j) && ((start.charAt(i) == 'L') != (i > j))) return false;
            i++; j++;
        }
        return true;
    }   
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第5篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~
