---
title: LC-Solution 2. Add Two Nums
date: 2022-10-03 20:43:59
tags: [算法]
categories: [Leetcode题解]
---

# [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

## 题目描述

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例1：

```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

示例2：

```
输入：l1 = [0], l2 = [0]
输出：[0]
```

示例3：

```
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

**提示：**

- 每个链表中的节点数在范围 `[1, 100]` 内
- `0 <= Node.val <= 9`
- 题目数据保证列表表示的数字不含前导零

## 解决方案

### 方法一：链表 + 数学 + 模拟

此题可直接理解为数学中的竖式相加

链表从左至右可以理解为从低位到高位，因此依次遍历链表，同一位置直接相加即可

设当前两个链表相同位置上的数分别为`x1`与`x2`，其进位值为`carry`

进行相加之后，`sum`为`x1 + x2 + carry`，则

- 结果链表的当前值`head.val = sum % 10`，添加至结果链表，链表前移
  
- 新的进位值`carry = sum // 10`
  

依次类推，继续前移链表节点，直至两链表遍历结束。

注意：遍历结束之后，需要判断最后的`carry`是否为1

- 若最后的`carry == 1`，则说明需要再向上进一位

#### Python3

```python
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        res = head = None
        carry = 0
        while l1 or l2:
            v1 = l1.val if l1 else 0
            v2 = l2.val if l2 else 0
            sum = v1 + v2 + carry
            if not res:
                res = head = ListNode(sum % 10)
            else:
                head.next = ListNode(sum % 10)
                head = head.next
            carry = sum // 10
            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next
        if carry > 0:
            head.next = ListNode(carry)
        return res
```

#### Java

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode res = null, head = null;
        int carry = 0;
        while(l1 != null || l2 != null) {
            int v1 = l1 == null ? 0 : l1.val;
            int v2 = l2 == null ? 0 : l2.val;
            int sum = v1 + v2 + carry;
            if(res == null) {
                res = head = new ListNode(sum % 10);
            } else {
                head.next = new ListNode(sum % 10);
                head = head.next;
            }
            carry = sum / 10;
            if(l1 != null) l1 = l1.next;
            if(l2 != null) l2 = l2.next;
        }
        if(carry > 0) {
            head.next = new ListNode(carry);
        }
        return res;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第2篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~