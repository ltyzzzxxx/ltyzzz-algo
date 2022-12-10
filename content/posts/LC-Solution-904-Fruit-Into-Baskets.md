---
title: LC-Solution 904. Fruit Into Baskets
date: 2022-10-17 12:39:57
tags: [算法]
categories: [Leetcode题解]
---

# [904. 水果成篮](https://leetcode.cn/problems/fruit-into-baskets/)

## 题目描述

你正在探访一家农场，农场从左到右种植了一排果树。这些树用一个整数数组 `fruits` 表示，其中 `fruits[i]` 是第 `i` 棵树上的水果 **种类** 。

你想要尽可能多地收集水果。然而，农场的主人设定了一些严格的规矩，你必须按照要求采摘水果：

-   你只有 **两个** 篮子，并且每个篮子只能装 **单一类型** 的水果。每个篮子能够装的水果总量没有限制。
-   你可以选择任意一棵树开始采摘，你必须从 **每棵** 树（包括开始采摘的树）上 **恰好摘一个水果** 。采摘的水果应当符合篮子中的水果类型。每采摘一次，你将会向右移动到下一棵树，并继续采摘。
-   一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。

给你一个整数数组 `fruits` ，返回你可以收集的水果的 **最大** 数目。

**示例 1：**

```
输入：fruits = [1,2,1]
输出：3
解释：可以采摘全部 3 棵树。
```

**示例 2：**

```
输入：fruits = [0,1,2,2]
输出：3
解释：可以采摘 [1,2,2] 这三棵树。
如果从第一棵树开始采摘，则只能采摘 [0,1] 这两棵树。
```

**示例 3：**

```
输入：fruits = [1,2,3,2,2]
输出：4
解释：可以采摘 [2,3,2,2] 这四棵树。
如果从第一棵树开始采摘，则只能采摘 [1,2] 这两棵树。
```

**示例 4：**

```
输入：fruits = [3,3,3,1,2,1,1,2,3,3,4]
输出：5
解释：可以采摘 [1,2,1,1,2] 这五棵树。
```

## 解决方案

### 方法一：滑动窗口

题目意思就是用两个篮子摘水果，每个篮子一旦确定好水果类型之后就不能更改，如果碰到了水果种类与两个篮子里的水果种类不同的树，就停止，此时摘到水果的数量等于开始位置到终止位置的长度。

采用滑动窗口：设 `i` 为滑动窗口的终止位置，`j` 为滑动窗口的起始位置，`i` 与 `j` 之间的水果种类只有两种。

使用哈希表记录某个种类水果对应的采摘数量。当哈希表长度超过2时，说明此时有3个水果种类，需要剔除掉其中一个。也就是说，此时需要滑动左端点 `j` 并减少 `j` 所对应水果的数量，直到哈希表长度为2，也就是剩余2个水果种类。

#### Python3

```python3
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        dic = defaultdict(int)
        j, ans = 0, 0
        for i, v in enumerate(fruits):
            dic[v] += 1
            while len(dic) > 2:
                dic[fruits[j]] -= 1
                if dic[fruits[j]] == 0:
                    dic.pop(fruits[j])
                j += 1
            ans = max(ans, i - j + 1)
        return ans
```

#### Java

```java
class Solution {
    public int totalFruit(int[] fruits) {
        Map<Integer, Integer> map = new HashMap<>();
        int j = 0;
        int ans = 0;
        for(int i = 0; i < fruits.length; i++) {
            map.put(fruits[i], map.getOrDefault(fruits[i], 0) + 1);
            while(map.size() > 2) {
                map.put(fruits[j], map.get(fruits[j]) - 1);
                if(map.get(fruits[j]) == 0) {
                    map.remove(fruits[j]);
                }
                j++;
            }
            ans = Math.max(ans, i - j + 1);
        }
        return ans;
    }
}
```

#### Go

```go
func totalFruit(fruits []int) int {
    cnt := map[int]int{}
    ans, j := 0, 0
    for i, x := range fruits {
        cnt[x] += 1
        for ; len(cnt) > 2; j++ {
            cnt[fruits[j]] -= 1
            if cnt[fruits[j]] == 0 {
                delete(cnt, fruits[j])
            }
        }
        ans = max(ans, i - j + 1)
    }
    return ans
}

func max(a, b int) int {
    if a > b {
        return a 
    }
    return b
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第26篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~