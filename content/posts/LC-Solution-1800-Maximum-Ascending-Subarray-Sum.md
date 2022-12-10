---
title: LC-Solution 1800. Maximum Ascending Subarray Sum
date: 2022-10-07 13:35:20
tags: [算法]
categories: [Leetcode题解]
---

[1800. 最大升序子数组和](https://leetcode.cn/problems/maximum-ascending-subarray-sum/)

## 题目描述

给你一个正整数组成的数组 `nums` ，返回 `nums` 中一个 **升序** 子数组的最大可能元素和。

子数组是数组中的一个连续数字序列。

已知子数组 `[numsl, numsl+1, ..., numsr-1, numsr]` ，若对所有 `i`（`l <= i < r`），`numsi` < `numsi+1` 都成立，则称这一子数组为 **升序** 子数组。注意，大小为 1 的子数组也视作 **升序** 子数组。

**示例1：**

```
输入：nums = [10,20,30,5,10,50]
输出：65
解释：[5,10,50] 是元素和最大的升序子数组，最大元素和为 65 。
```

**示例2：**

```
输入：nums = [10,20,30,40,50]
输出：150
解释：[10,20,30,40,50] 是元素和最大的升序子数组，最大元素和为 150 。
```

**示例3：**

```
输入：nums = [12,17,15,13,10,11,12]
输出：33
解释：[10,11,12] 是元素和最大的升序子数组，最大元素和为 33 。 
```

**示例4：**

```
输入：nums = [100,10,1]
输出：100
```

**提示：**

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

## 解决方案

### 方法一：简单模拟

遍历数组，比较相邻元素之间是否满足升序条件。

用 `s` 记录当前升序子数组的和，用 `ans` 记录最终最大的和。

- 若当前元素大于前一个元素（ `nums[i] > nums[i - 1`] ），将当前元素 `nums[i]` 累加到 `s` 中，并更新 `ans = max(ans, s)`
  
- 否则，将 `s` 重置为当前元素，即 `s = nums[i]`
  

#### Python3

```python
class Solution:
    def maxAscendingSum(self, nums: List[int]) -> int:
        ans = s = nums[0]
        for i in range(1,len(nums)):
            if nums[i] > nums[i - 1]:
                s += nums[i]
                ans = max(ans, s)
            else:
                s = nums[i]
        return ans      
```

#### Java

```java
class Solution {
    public int maxAscendingSum(int[] nums) {
        int ans = nums[0], sum = nums[0];
        for(int i = 1; i < nums.length; i++) {
            if(nums[i] > nums[i - 1]) {
                sum += nums[i];
                ans = Math.max(ans, sum);
            } else {
                sum = nums[i];
            }
        }
        return ans;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第14篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~