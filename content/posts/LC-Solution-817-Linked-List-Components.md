---
title: LC-Solution 817. Linked List Components
date: 2022-10-12 12:29:51
tags: [算法]
categories: [Leetcode题解]
---

# [817. 链表组件](https://leetcode.cn/problems/linked-list-components/)

## 题目描述

给定链表头结点 `head`，该链表上的每个结点都有一个 **唯一的整型值** 。同时给定列表 `nums`，该列表是上述链表中整型值的一个子集。

返回列表 `nums` 中组件的个数，这里对组件的定义为：链表中一段最长连续结点的值（该值必须在列表 `nums` 中）构成的集合。

**示例 1：**

```
输入: head = [0,1,2,3], nums = [0,1,3]
输出: 2
解释: 链表中,0 和 1 是相连接的，且 nums 中不包含 2，所以 [0, 1] 是 nums 的一个组件，同理 [3] 也是一个组件，故返回 2。
```

**示例 2：**

```
输入: head = [0,1,2,3,4], nums = [0,3,1,4]
输出: 2
解释: 链表中，0 和 1 是相连接的，3 和 4 是相连接的，所以 [0, 1] 和 [3, 4] 是两个组件，故返回 2。
```

**提示：**

- 链表中节点数为`n`
- `1 <= n <= 104`
- `0 <= Node.val < n`
- `Node.val` 中所有值 **不同**
- `1 <= nums.length <= n`
- `0 <= nums[i] < n`
- `nums` 中所有值 **不同**

## 解决方案

### 方法一：模拟 + 哈希表 + 链表遍历

分析题意可知，需要返回链表中连通量的个数（连通量中的值必须在 `nums` 中）

因此我们可以通过遍历列表，判断当前节点 `cur.val` 以及其前驱节点 `pre.val` 是否存在于 `nums` 中，如下分类讨论：

1. 若 `cur.val` 在 `nums` 中，但 `pre.val` 不在 `nums` 中：
  
  说明此时产生了新的连通分量，需要进行计数
  
2. 若 `cur.val` 不在 `nums` 中，`pre.val` 也不在 `nums` 中：没有新的连通分量产生
  
3. 若 `cur.val` 不在 `nums` 中，但`pre.val` 在 `nums` 中：没有新的连通分量产生
  
4. 若 `cur.val` 在 `nums` 中，`pre.val` 也在 `nums` 中：
  
  没有新的连通分量产生，`cur` 归属于前一个连通分量集合中
  

因此，只有第一种情况下，我们需要进行计数，其他情况不用体现在代码中。

为了提高判断链表节点值是否在 `nums` 中，我们将其转换为 `set` 集合，更加高效。

#### Python3

```python
class Solution:
    def numComponents(self, head: Optional[ListNode], nums: List[int]) -> int:
        cur = head
        ans, pre = 0, ListNode(-1)
        s = set(nums)
        while cur:
            if cur.val in s and pre.val not in s:
                    ans += 1
            pre, cur = cur, cur.next
        return ans
```

#### Java

```java
class Solution {
    public int numComponents(ListNode head, int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(int n : nums) set.add(n);
        ListNode cur = head, pre = new ListNode(-1);
        int ans = 0;
        while(cur != null) {
            if(set.contains(cur.val) && !set.contains(pre.val)) {
                ans++;
            }
            pre = cur;
            cur = cur.next;
        }
        return ans;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第23篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~