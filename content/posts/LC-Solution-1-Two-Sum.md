---
title: LC-Solution 1. Two Sum
date: 2022-10-03 18:48:37
tags: [算法]
categories: [Leetcode题解]
---

# [1.两数之和](https://leetcode.cn/problems/two-sum/)

## 题目描述

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`*  的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重lt复出现。

你可以按任意顺序返回答案。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

进阶：你可以想出一个时间复杂度小于 `O(n2)` 的算法吗？

## 解决方案

### 方法一：哈希表

对数组进行遍历，用哈希表存放数组值及其对应的下标值。

- `key` => 元素值， `value` => 元素值对应的下标值

当发现`target - nums[i]` 即 `目标值与当前值之差` 是否在哈希表中出现过

- 若出现过，则说明找到了目标值，直接返回即可
  
- 若未出现，继续遍历
  

核心思路为`空间换时间`，若不采用哈希表，则双重for循环朴素解法时间复杂度为`O(n^2)`

在循环遍历的过程中确定`nums[i]`为当前数，然后在哈希表中查找该数之前是否存在一个数为`target - nums[i]`。

#### Python3

```python
class Solution:
    def twoSum(self, nums: List[int], t: int) -> List[int]:
        dic = defaultdict(int)
        for i, v in enumerate(nums):
            x = t - v
            if x in dic:
                return [dic[x], i]
            dic[v] = i
```

#### Java

```java
class Solution {
    public int[] twoSum(int[] nums, int t) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++) {
            if(map.containsKey(t - nums[i])) return new int[]{map.get(t - nums[i]), i};
            map.put(nums[i], i);
        }
        return new int[]{};
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第1篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~