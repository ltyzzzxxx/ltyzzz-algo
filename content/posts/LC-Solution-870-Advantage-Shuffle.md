---
title: LC-Solution 870. Advantage Shuffle
date: 2022-10-08 15:43:39
tags: [算法]
categories: [Leetcode题解]
---

# [870. Advantage Shuffle](https://leetcode.cn/problems/advantage-shuffle/)

## 题目描述

给定两个大小相等的数组 `nums1` 和 `nums2`，`nums1` 相对于 `nums` 的优势可以用满足 `nums1[i]` > `nums2[i]` 的索引 `i` 的数目来描述。

返回 `nums1` 的任意排列，使其相对于 `nums2` 的优势最大化。

**示例 1：**

```
输入：nums1 = [2,7,11,15], nums2 = [1,10,4,11]
输出：[2,11,7,15]
```

**示例 2：**

```
输入：nums1 = [12,24,8,32], nums2 = [13,25,32,11]
输出：[24,32,8,12]
```

**提示：**

- 1 <= nums1.length <= 105
  
- nums2.length == nums1.length
  
- 0 <= nums1[i], nums2[i] <= 109
  

## 解决方案

### 方法一：田忌赛马 -> 贪心 + 双指针

该方案采用的是 [灵神]([力扣](https://leetcode.cn/problems/advantage-shuffle/solution/tian-ji-sai-ma-by-endlesscheng-yxm6/)) 的题解，下面我说一下我的理解

题目要求使 `nums1` 数组 **“战胜”** `nums2` 数组的次数最大化

通过此很容易想到贪心思路，对 `nums1` 与 `nums2` 分别进行排序，

然后对比 `nums1` 当前最小值 与 `nums2` 当前最小值

- 若 `nums1` 的当前最小值大于 `nums2` 的当前最小值，则 `nums1` 获胜
  
- 否则，说明 `nums1` 的当前最小值无法战胜 `nums2` 的所有值，所以让 `nums1` 的当前最小值与 `nums2` 的当前最大值作战（即当“炮灰”），这样可以抵消掉 `nums2` 的最大值。
  

这就是典型的 `田忌赛马` 思路，用下等马去战胜上等马！

- `nums1` 的下等马无法战胜 `nums2` 的下等马时，就让其当炮灰去对战 `nums2` 的上等马
  

每一个 “作战回合” 结束之后，需要将本回合用过的元素摒弃掉。

而 `nums2` 的最小值（即数组尾元素）或最大值（即数组首元素）均可能被使用，所以本题采用 `首尾双指针` 思路，逐渐缩小问题规模，最终得到全部对应关系。

但在代码具体实现过程中，如果对两个数组均进行排序，则丢失了原本的 ”作战“ 顺序，所以只对 `nums1` 进行排序。用额外的下标数组 `ids`，根据 `nums2` 的元素大小，对其下标进行排序。`ids[0]` 对应 `nums2` 第一小值下标，`ids[1]` 对应 `nums2` 第二小值下标...

通过 `ids` 数组，就可以知道当前 `nums2` 下标所要对应的 `nums1` 元素。

感觉这道题直接看代码更容易理解。

#### Python3

```python
class Solution:
    def advantageCount(self, nums1: List[int], nums2: List[int]) -> List[int]:
        n = len(nums1)
        ans = [0] * n
        nums1.sort()
        ids = sorted(range(n), key=lambda i: nums2[i])
        left, right = 0, n - 1
        for x in nums1:
            if x > nums2[ids[left]]:
                ans[ids[left]] = x  
                left += 1
            else:
                ans[ids[right]] = x  
                right -= 1
        return ans
```

#### Java

注意：Java采用比较器排序时，只能使用包装类Integer

```java
class Solution {
    public int[] advantageCount(int[] nums1, int[] nums2) {
        int len = nums2.length;
        Integer[] ids = new Integer[len];
        for(int i = 0; i < len; i++) ids[i] = i;
        Arrays.sort(nums1);
        Arrays.sort(ids, (a, b) -> nums2[a] - nums2[b]);
        int p = 0, q = len - 1;
        int[] ans = new int[len];
        for(int x : nums1) {
            if(x > nums2[ids[p]]) ans[ids[p++]] = x;
            else ans[ids[q--]] = x;
        }
        return ans;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第15篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~