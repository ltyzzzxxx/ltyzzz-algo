---
title: LC-Solution 1833. Maximum Ice Cream Bars
date: 2022-10-11 15:17:27
tags: [算法]
categories: [Leetcode题解]
---

# [1833. 雪糕的最大数量](https://leetcode.cn/problems/maximum-ice-cream-bars/)

## 题目描述

夏日炎炎，小男孩 Tony 想买一些雪糕消消暑。

商店中新到 `n` 支雪糕，用长度为 `n` 的数组 `costs` 表示雪糕的定价，其中 `costs[i]` 表示第 `i` 支雪糕的现金价格。Tony 一共有 `coins` 现金可以用于消费，他想要买尽可能多的雪糕。

给你价格数组 `costs` 和现金量 `coins` ，请你计算并返回 Tony 用 `coins` 现金能够买到的雪糕的 **最大数量** 。

**注意：** Tony 可以按任意顺序购买雪糕。

**示例 1：**

```
输入：costs = [1,3,2,4,1], coins = 7
输出：4
解释：Tony 可以买下标为 0、1、2、4 的雪糕，总价为 1 + 3 + 2 + 1 = 7
```

**示例 2：**

```
输入：costs = [10,6,8,7,7,8], coins = 5
输出：0
解释：Tony 没有足够的钱买任何一支雪糕。
```

**示例 3：**

```
输入：costs = [1,6,3,1,2,5], coins = 20
输出：6
解释：Tony 可以买下所有的雪糕，总价为 1 + 6 + 3 + 1 + 2 + 5 = 18 。
```

**提示：**

- `costs.length == n`
- `1 <= n <= 105`
- `1 <= costs[i] <= 105`
- `1 <= coins <= 108`

## 解决方案

### 方法一：贪心

用有限的钱，买到最多的雪糕。

从此很容易可以想到，对于价格便宜的雪糕，购买优先级高。

因此，对雪糕的价格数组 `costs` 进行排序。

每个雪糕只能买一次，因此遍历 `costs` 数组，用 `cnt` 计数买到雪糕的数量。

当将 `coins` 花完的时候，便可以得到可以购买的最大数量。

#### Python3

```python
class Solution:
    def maxIceCream(self, costs: List[int], coins: int) -> int:
        costs.sort()
        cnt = 0
        for c in costs:
            if coins - c >= 0:
                coins -= c
                cnt += 1
            else:
                break
        return cnt
```

#### Java

```java
class Solution {
    public int maxIceCream(int[] costs, int coins) {
        Arrays.sort(costs);
        int cnt = 0;
        for(int c : costs) {
            if(coins - c >= 0) {
                coins -= c;
                cnt++;
            } else {
                break;
            }
        }
        return cnt;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第20篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~
