---
title: LC-Solution 1441. Build an Array With Stack Operations
date: 2022-10-15 19:47:08
tags: [算法]
categories: [Leetcode题解]
---

# [1441. 用栈操作构建数组](https://leetcode.cn/problems/build-an-array-with-stack-operations/)

## 题目描述

给你一个数组 `target` 和一个整数 `n`。每次迭代，需要从 `list = { 1 , 2 , 3 ..., n }` 中依次读取一个数字。

请使用下述操作来构建目标数组 `target` ：

-   `"Push"`：从 `list` 中读取一个新元素， 并将其推入数组中。
-   `"Pop"`：删除数组中的最后一个元素。
-   如果目标数组构建完成，就停止读取更多元素。

题目数据保证目标数组严格递增，并且只包含 `1` 到 `n` 之间的数字。

请返回构建目标数组所用的操作序列。如果存在多个可行方案，返回任一即可。

**示例 1：**

```
输入：target = [1,3], n = 3
输出：["Push","Push","Pop","Push"]
解释： 
读取 1 并自动推入数组 -> [1]
读取 2 并自动推入数组，然后删除它 -> [1]
读取 3 并自动推入数组 -> [1,3]
```

**示例 2：**

```
输入：target = [1,2,3], n = 3
输出：["Push","Push","Push"]
```

**示例 3：**

```
输入：target = [1,2], n = 4
输出：["Push","Push"]
解释：只需要读取前 2 个数字就可以停止。
```

**提示：**

-   `1 <= target.length <= 100`
-   `1 <= n <= 100`
-   `1 <= target[i] <= n`
-   `target` 严格递增

## 解决方案

### 方法一：简单模拟

题目给定的条件：`target` 数组严格递增升序且只包含1~n之间的数字

1.   对于 `target` 中缺少的数字，分类讨论
     1.   该数字小于 `target[-1]`（数组最后一个元素），需要进行 'Push' + 'Pop' 操作
     2.   该数字大于 `target[-1]`（数组最后一个元素），无需进行操作，因为该数已经不在 `target` 内了
2.   对于 `target` 中存在的数字，我只需要进行 'Push' 操作

根据上述分析可知，我们根本不需要用到题目给定的 n，因为 n 中大于 `target[-1]` 的数不需要处理，而小于 `target[-1]` 的数只需要遍历 `target` 数组即可

因此，遍历 `target` 数组，找出 `target` 缺少的数字。通过初始化 `j = 1`，判断当前遍历的数是否等于 `j` 来判断是否缺少该数。

直接看代码更好理解！

#### Python3

```python3
class Solution:
    def buildArray(self, target: List[int], n: int) -> List[str]:
        ans = []
        j = 1
        for i in range(len(target)):
            while target[i] != j:
                ans.append('Push')
                ans.append('Pop')
                j += 1
            ans.append('Push')
            j += 1 # 当target[i] == j时，处理完之后还需要在自增一次j
        return ans
```

#### Java

```java
class Solution {
    public List<String> buildArray(int[] target, int n) {
        int len = target.length;
        List<String> ans = new ArrayList<>();
        int j = 1;
        for(int i = 0; i < len; i++) {
            while(target[i] != j) {
                ans.add("Push");
                ans.add("Pop");
                j++;
            }
            ans.add("Push")
            j++; // 当target[i] == j时，处理完之后还需要在自增一次j
        }
        return ans;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第25篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~
