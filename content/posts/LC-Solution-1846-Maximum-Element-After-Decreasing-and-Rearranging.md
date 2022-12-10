---
title: LC-Solution 1846. Maximum Element After Decreasing and Rearranging
date: 2022-10-11 15:17:41
tags: [算法]
categories: [Leetcode题解]
---

# [1846. 减小和重新排列数组后的最大元素](https://leetcode.cn/problems/maximum-element-after-decreasing-and-rearranging/)

## 题目描述

给你一个正整数数组 `arr` 。请你对 `arr` 执行一些操作（也可以不进行任何操作），使得数组满足以下条件：

- `arr` 中 **第一个** 元素必须为 `1`
  
- 任意相邻两个元素的差的绝对值 **小于等于** `1` ，也就是说，对于任意的 `1 <= i < arr.length` （**数组下标从 0 开始**），都满足 `abs(arr[i] - arr[i - 1]) <= 1` 。`abs(x)` 为 `x` 的绝对值。
  

你可以执行以下 2 种操作任意次：

- **减小** `arr` 中任意元素的值，使其变为一个 **更小的正整数** 。
- **重新排列** `arr` 中的元素，你可以以任意顺序重新排列。

请你返回执行以上操作后，在满足前文所述的条件下，`arr` 中可能的 **最大值** 。

**示例1：**

```
输入：arr = [2,2,1,2,1]
输出：2
解释：
我们可以重新排列 arr 得到 [1,2,2,2,1] ，该数组满足所有条件。
arr 中最大元素为 2 。
```

**示例 2：**

```
输入：arr = [100,1,1000]
输出：3
解释：
一个可行的方案如下：
1. 重新排列 arr 得到 [1,100,1000] 。
2. 将第二个元素减小为 2 。
3. 将第三个元素减小为 3 。
现在 arr = [1,2,3] ，满足所有条件。
arr 中最大元素为 3 。
```

**示例 3：**

```
输入：arr = [1,2,3,4,5]
输出：5
解释：数组已经满足所有条件，最大元素为 5 。
```

**提示：**

- `1 <= arr.length <= 105`
- `1 <= arr[i] <= 109`

## 解决方案

### 方法一：贪心

分析题意：题目要求通过 `减法运算` 或 `重排序` 两种方法，使得数组的两两元素之差的绝对值小于等于1（`abs(arr[i] - arr[i - 1]) <= 1`），且 `arr[0] = 1`

于是很容易想到用贪心思想解决本题

1. 允许重排序且题目最终要求返回数组最大值：最终返回结果与数组顺序无关，所以我们可以直接对数组 `arr` 进行排序。
  
2. `arr[0] = 1`：从此可以确定对 `arr` 进行升序排序
  
3. 可以做减法运算：从此条件可以进一步简化题目。既然我们之前已经确定了采用升序排序，那么可以得知此条件始终成立 `arr[i] >= arr[i - 1]`。
  
  所以我们遍历 `arr` 数组时：
  
  1. 无需计算元素之差的绝对值
    
  2. 当不满足元素之差小于等于1的条件（ `arr[i] > arr[i] + 1` ）时，只需 `arr[i] = arr[i -1] + 1`，使较大的值等于较小的值加1，相当于对较大的值做减法运算
    

#### Python3

```python
class Solution:
    def maximumElementAfterDecrementingAndRearranging(self, arr: List[int]) -> int:
        arr.sort()
        arr[0] = 1
        for i in range(1, len(arr)):
            if arr[i] - arr[i - 1] > 1:
                arr[i] = arr[i - 1] + 1
        return arr[-1]
```

#### Java

```java
class Solution {
    public int maximumElementAfterDecrementingAndRearranging(int[] arr) {
        Arrays.sort(arr);
        arr[0] = 1;
        for(int i = 1; i < arr.length; i++) {
            if(arr[i] - arr[i - 1] > 1) {
               arr[i] = arr[i - 1] + 1;
            }
        }
        return arr[arr.length - 1];
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第21篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~