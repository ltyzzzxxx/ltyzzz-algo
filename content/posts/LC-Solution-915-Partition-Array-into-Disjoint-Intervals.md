---
title: LC Solution 915. Partition Array into Disjoint Intervals
date: 2022-10-24 14:06:48
tags: [算法]
categories: [Leetcode题解]
---

# [915. 分割数组](https://leetcode.cn/problems/partition-array-into-disjoint-intervals/)

## 题目描述

给定一个数组 `nums` ，将其划分为两个连续子数组 `left` 和 `right`， 使得：

-   `left` 中的每个元素都小于或等于 `right` 中的每个元素。
-   `left` 和 `right` 都是非空的。
-   `left` 的长度要尽可能小。

*在完成这样的分组后返回 `left` 的 **长度*** 。

用例可以保证存在这样的划分方法。

**示例 1：**

```
输入：nums = [5,0,3,8,6]
输出：3
解释：left = [5,0,3]，right = [8,6]
```

**示例 2：**

```
输入：nums = [1,1,1,0,6,12]
输出：4
解释：left = [1,1,1,0]，right = [6,12]
```

**提示：**

-   `2 <= nums.length <= 105`
-   `0 <= nums[i] <= 106`
-   可以保证至少有一种方法能够按题目所描述的那样对 `nums` 进行划分。

## 解决方案

### 方案一：前缀与后缀

分析题意，核心思想就是找出分界点，且该分界点满足：**左侧子数组的最大值** 小于等于 **右侧子数组的最小值**

而且根据题目数据量 `10e5` 可知，无法使用O(n2)复杂度的算法即暴力解法。

据此便想到：两次遍历统计前缀子数组的最大值与后缀子数组的最小值，分别记录在两个数组中。最后再遍历一次统计好的数组，找出分界点。

#### Python3

```python
class Solution:
    def partitionDisjoint(self, nums: List[int]) -> int:    
        n = len(nums)
        min_nums, max_nums = [0] * n, [0] * n # 初始化前后缀数组
        min_num, max_num = 10 ** 7, -1
        for i in range(n):
            max_num = max(nums[i], max_num)
            max_nums[i] = max_num
        for i in range(n - 1, -1, -1):
            min_num = min(nums[i], min_num)
            min_nums[i] = min_num
        for i in range(n - 1):
            # 小于等于的情况下，找到第一个满足的分界点，题目要求left尽可能小，于是直接返回
            if max_nums[i] <= min_nums[i + 1]:
                return i + 1
        return -1
```

#### Java

```java
class Solution {
    public int partitionDisjoint(int[] nums) {
        int n = nums.length;
        int[] minNums = new int[n], maxNums = new int[n];
        int minNum = Integer.MAX_VALUE, maxNum = Integer.MIN_VALUE;
        for(int i = 0; i < n; i++) {
            maxNum = Math.max(nums[i], maxNum);
            maxNums[i] = maxNum;
        }
        for(int i = n - 1; i >= 0; i--) {
            minNum = Math.min(nums[i], minNum);
            minNums[i] = minNum;
        }
        for(int i = 0; i < n - 1; i++) {
            if(maxNums[i] <= minNums[i + 1]) return i + 1;
        }
        return -1;
    }
}
```

#### Go

```go
func partitionDisjoint(nums []int) int {
    n := len(nums)
    minNums, maxNums := make([]int, n), make([]int, n)
    minNum, maxNum := math.MaxInt32, -1
    for i := 0; i < n; i++ {
        if nums[i] > maxNum {
            maxNum = nums[i]
        }
        maxNums[i] = maxNum
    }
    for i := n - 1; i >= 0; i-- {
        if nums[i] < minNum {
            minNum = nums[i]
        }
        minNums[i] = minNum
    }
    for i := 0; i < n - 1; i++ {
        if maxNums[i] <= minNums[i + 1] {
            return i + 1
        }
    }
    return -1
}
```

### 方案二：优化前缀与后缀

我们可以在第一次遍历的时候，统计后缀子数组的最小值，将其保存在一个数组中。

第二次从前向后遍历，一边保存当前前缀子数组的最大值，一边判断并寻找分界点。

优化之后，时间效率上可以提升25%左右。

#### Python3

```python
class Solution:
    def partitionDisjoint(self, nums: List[int]) -> int:    
        n = len(nums)
        min_nums = [0] * n
        min_num, max_num = 10 ** 7, -1
        for i in range(n - 1, -1, -1):
            min_num = min(nums[i], min_num)
            min_nums[i] = min_num
        for i in range(n - 1):
            max_num = max(nums[i], max_num)
            if max_num <= min_nums[i + 1]:
                return i + 1
        return -1
```

#### Java

```java
class Solution {
    public int partitionDisjoint(int[] nums) {
        int n = nums.length;
        int[] minNums = new int[n];
        int minNum = Integer.MAX_VALUE, maxNum = Integer.MIN_VALUE;
        for(int i = n - 1; i >= 0; i--) {
            minNum = Math.min(nums[i], minNum);
            minNums[i] = minNum;
        }
        for(int i = 0; i < n - 1; i++) {
            maxNum = Math.max(nums[i], maxNum);
            if(maxNum <= minNums[i + 1]) return i + 1;
        }
        return -1;
    }
}
```

#### Go

```go
func partitionDisjoint(nums []int) int {
    n := len(nums)
    minNums := make([]int, n)
    minNum, maxNum := math.MaxInt32, -1
    for i := n - 1; i >= 0; i-- {
        if nums[i] < minNum {
            minNum = nums[i]
        }
        minNums[i] = minNum
    }
    for i := 0; i < n - 1; i++ {
        if nums[i] > maxNum {
            maxNum = nums[i]
        }
        if maxNum <= minNums[i + 1] {
            return i + 1
        }
    }
    return -1
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第31篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~