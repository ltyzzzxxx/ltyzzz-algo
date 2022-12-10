---
title: LC-Solution 1700. Number of Students Unable to Eat Lunch
date: 2022-10-19 14:15:01
tags: [算法]
categories: [Leetcode题解]
---

# [1700. 无法吃午餐的学生数量](https://leetcode.cn/problems/number-of-students-unable-to-eat-lunch/)

## 题目描述

学校的自助午餐提供圆形和方形的三明治，分别用数字 `0` 和 `1` 表示。所有学生站在一个队列里，每个学生要么喜欢圆形的要么喜欢方形的。

餐厅里三明治的数量与学生的数量相同。所有三明治都放在一个 **栈** 里，每一轮：

-   如果队列最前面的学生 **喜欢** 栈顶的三明治，那么会 **拿走它** 并离开队列。
-   否则，这名学生会 **放弃这个三明治** 并回到队列的尾部。

这个过程会一直持续到队列里所有学生都不喜欢栈顶的三明治为止。

给你两个整数数组 `students` 和 `sandwiches` ，其中 `sandwiches[i]` 是栈里面第 `i` 个三明治的类型（`i = 0` 是栈的顶部）， `students[j]` 是初始队列里第 `j` 名学生对三明治的喜好（`j = 0` 是队列的最开始位置）。请你返回无法吃午餐的学生数量。

**示例 1：**

```
输入：students = [1,1,0,0], sandwiches = [0,1,0,1]
输出：0 
```

**示例 2：**

```
输入：students = [1,1,1,0,0,1], sandwiches = [1,0,0,0,1,1]
输出：3
```

**提示：**

-   `1 <= students.length, sandwiches.length <= 100`
-   `students.length == sandwiches.length`
-   `sandwiches[i]` 要么是 `0` ，要么是 `1` 。
-   `students[i]` 要么是 `0` ，要么是 `1` 。

## 解决方案

### 方法一：队列 + 栈模拟

1.   学生以队列形式取三明治
     -   如果队列头的学生喜欢当前三明治，则取走三明治之后出队列
     -   否则，该学生返回队列尾，继续排队取三明治
2.   三明治以栈的形式被取走
     -   如果栈顶三明治被学生喜欢，则出栈
     -   否则，保持不变

如此分析不难得出，如果队列中没有一个学生喜欢栈顶的三明治，那么循环将终止，当前队列中的所有学生将无法吃到三明治。

#### Python3

```python
class Solution:
    def countStudents(self, sts: List[int], sas: List[int]) -> int:
        while sts and sas and sas[0] in sts:
            if sts[0] == sas[0]:
                sts.pop(0)
                sas.pop(0)
            else:
                st = sts.pop(0)
                sts.append(st)
        return len(sts)
```

#### Java

```java
class Solution {
    public int countStudents(int[] sts, int[] sas) {
        Deque<Integer> queue = new ArrayDeque<>();
        Deque<Integer> stack = new ArrayDeque<>();
        for(int v : sts) queue.addLast(v);
        for(int v : sas) stack.addLast(v);
        while(!queue.isEmpty() && !stack.isEmpty() && queue.contains(stack.peekFirst())) {
            if(queue.peekFirst() == stack.peekFirst()) {
                queue.pollFirst();
                stack.pollFirst();
            } else {
                int v = queue.pollFirst();
                queue.addLast(v);
            }
        }
        return queue.size();
    }
}
```

### 方法二：计数

根据之前方法一中的分析，发现关键在于栈顶的三明治是否被队列中的学生所喜欢，如果没有一个学生喜欢，则可以得出最终答案

于是可以采用计数方式，用 `cnt` 统计喜欢两种三明治的学生人数。

遍历三明治数组，递减 `cnt[v]`，当 `cnt[v] == -1`，也就是没有学生喜欢当前三明治，则直接返回剩余学生数量即可。

#### Python3

```python
class Solution:
    def countStudents(self, sts: List[int], sas: List[int]) -> int:
        cnt = Counter(sts)
        for i in range(len(sas)):
            cnt[sas[i]] -= 1
            # 这里是先减后判断，所以是-1
            if cnt[sas[i]] == -1:
                return cnt[sas[i] ^ 1]
        return 0
```

#### Java

```java
class Solution {
    public int countStudents(int[] sts, int[] sas) {
        int[] cnt = new int[2];
        for(int v : sts) cnt[v]++;
        for(int i = 0; i < sas.length; i++) {
            if(--cnt[sas[i]] == -1) return sas.length - i;
        }
        return 0;
    }
}
```

#### Go

```go
func countStudents(sts []int, sas []int) int {
    cnt := make([]int, 2)
    for _, v := range sts {
        cnt[v]++
    }
    for i, v := range sas {
        cnt[v]--
        if cnt[v] == -1 {
            return len(sas) - i
        }
    }
    return 0
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第27篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~
