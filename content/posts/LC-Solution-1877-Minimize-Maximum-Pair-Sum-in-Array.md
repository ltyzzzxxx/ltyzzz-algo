---
title: LC-Solution 1877. Minimize Maximum Pair Sum in Array
date: 2022-10-11 15:17:52
tags: [算法]
categories: [Leetcode题解]
---

# [1877. 数组中最大数对和的最小值](https://leetcode.cn/problems/minimize-maximum-pair-sum-in-array/)

## 题目描述

一个数对 `(a,b)` 的 **数对和** 等于 `a + b` 。**最大数对和** 是一个数对数组中最大的 **数对和** 。

- 比方说，如果我们有数对 `(1,5)` ，`(2,3)` 和 `(4,4)`，**最大数对和** 为 `max(1+5, 2+3, 4+4) = max(6, 5, 8) = 8` 。

给你一个长度为 **偶数** `n` 的数组 `nums` ，请你将 `nums` 中的元素分成 `n / 2` 个数对，使得：

- `nums` 中每个元素 **恰好** 在 **一个** 数对中，且
- **最大数对和** 的值 **最小** 。

请你在最优数对划分的方案下，返回最小的 **最大数对和** 。

**示例 1：**

```
输入：nums = [3,5,2,3]
输出：7
解释：数组中的元素可以分为数对 (3,3) 和 (5,2) 。
最大数对和为 max(3+3, 5+2) = max(6, 7) = 7 。
```

**示例 2：**

```
输入：nums = [3,5,4,2,4,6]
输出：8
解释：数组中的元素可以分为数对 (3,5)，(4,4) 和 (6,2) 。
最大数对和为 max(3+5, 4+4, 6+2) = max(8, 8, 8) = 8 。
```

**提示：**

- `n == nums.length`
- `2 <= n <= 105`
- `n` 是 **偶数** 。
- `1 <= nums[i] <= 105`

## 解决方案

### 方法一：贪心 + 双指针

分析题意可知：题目给定了一个偶数长度的 `nums` 数组，将其两两分组组成数对，使得最终构成的数对数组中的最大数对和尽可能的小。

数对和意思是：一个数对中的两个数之和

为了使得最大的数对和尽可能地小，直观判断便是采用贪心思想，使得数组中的最小值与最大值组队，第二小值与第二大值组队 ... 依次类推。最终得到的数对数组便可符合这一条件。

于是根据这一贪心思想，很容易想到算法实现方案 -> 排序双指针，遍历数组

#### Python3

```python
class Solution:
    def minPairSum(self, nums: List[int]) -> int:
        nums.sort()
        i, j = 0, len(nums) - 1
        ans = 0
        while i < j:
            ans = max(ans, nums[i] + nums[j])
            i, j = i + 1, j - 1
        return ans
```

采用推导式进一步简化代码

```python
class Solution:
    def minPairSum(self, nums: List[int]) -> int:
        nums.sort()
        return max(nums[i] + nums[len(nums) - 1 - i] for i in range(len(nums)) if i < len(nums) // 2)
```

#### Java

```java
class Solution {
    public int minPairSum(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);
        int l = 0, r = n - 1;
        int ans = 0;
        while(l < r) {
            ans = Math.max(ans, nums[l] + nums[r]);
            l++;
            r--;
        }
        return ans;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第22篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~