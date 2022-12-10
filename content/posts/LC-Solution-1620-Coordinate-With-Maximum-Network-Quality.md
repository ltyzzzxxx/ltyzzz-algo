---
title: LC Solution 1620. Coordinate With Maximum Network Quality
date: 2022-11-02 16:46:41
tags: [算法]
categories: [Leetcode题解]
---

# [1620. 网络信号最好的坐标](https://leetcode.cn/problems/coordinate-with-maximum-network-quality/)

## 题目描述

给你一个数组 `towers` 和一个整数 `radius` 。

数组 `towers` 中包含一些网络信号塔，其中 `towers[i] = [xi, yi, qi]` 表示第 `i` 个网络信号塔的坐标是 `(xi, yi)` 且信号强度参数为 `qi` 。所有坐标都是在 X-Y 坐标系内的 **整数** 坐标。两个坐标之间的距离用 **欧几里得距离** 计算。

整数 `radius` 表示一个塔 **能到达** 的 **最远距离** 。如果一个坐标跟塔的距离在 `radius` 以内，那么该塔的信号可以到达该坐标。在这个范围以外信号会很微弱，所以 `radius` 以外的距离该塔是 **不能到达的** 。

如果第 `i` 个塔能到达 `(x, y)` ，那么该塔在此处的信号为 `⌊qi / (1 + d)⌋` ，其中 `d` 是塔跟此坐标的距离。一个坐标的 **信号强度** 是所有 **能到达** 该坐标的塔的信号强度之和。

请你返回数组 `[cx, cy]` ，表示 **信号强度** 最大的 **整数** 坐标点 `(cx, cy)` 。如果有多个坐标网络信号一样大，请你返回字典序最小的 **非负** 坐标。

**注意：**

-   坐标 `(x1, y1)` 字典序比另一个坐标 `(x2, y2)` 小，需满足以下条件之一：
    -   要么 `x1 < x2` ，
    -   要么 `x1 == x2` 且 `y1 < y2` 。
-   `⌊val⌋` 表示小于等于 `val` 的最大整数（向下取整函数）。

示例 1：

```
输入：towers = [[1,2,5],[2,1,7],[3,1,9]], radius = 2
输出：[2,1]
解释：
坐标 (2, 1) 信号强度之和为 13
- 塔 (2, 1) 强度参数为 7 ，在该点强度为 ⌊7 / (1 + sqrt(0)⌋ = ⌊7⌋ = 7
- 塔 (1, 2) 强度参数为 5 ，在该点强度为 ⌊5 / (1 + sqrt(2)⌋ = ⌊2.07⌋ = 2
- 塔 (3, 1) 强度参数为 9 ，在该点强度为 ⌊9 / (1 + sqrt(1)⌋ = ⌊4.5⌋ = 4
没有别的坐标有更大的信号强度。
```

示例 2：

```
输入：towers = [[23,11,21]], radius = 9
输出：[23,11]
解释：由于仅存在一座信号塔，所以塔的位置信号强度最大。
```

示例 3：

```
输入：towers = [[1,2,13],[2,1,7],[0,1,9]], radius = 2
输出：[1,2]
解释：坐标 (1, 2) 的信号强度最大。
```

提示：

-   `1 <= towers.length <= 50`
-   `towers[i].length == 3`
-   `0 <= xi, yi, qi <= 50`
-   `1 <= radius <= 50`

## 解决方案

### 方法一：枚举

题目要求找到一个坐标，使得该点到所有信号塔的信号强度之和最大。

题目给出的坐标范围最小为 `(0, 0)`，最大为 `(50, 50)`，而灯塔数量最多为50。即使采用三重 `for` 循环去遍历求解，计算次数也不会超过10e8，不存在TLE问题。因此选择枚举求解。

#### Python3

```python3
class Solution:
    def bestCoordinate(self, towers: List[List[int]], radius: int) -> List[int]:
        max_v = 0
        ans = [0, 0]
        for i in range(51):
            for j in range(51):
                cur = 0
                for x, y, q in towers:
                    d = ((x - i) ** 2 + (y - j) ** 2) ** 0.5
                    if d <= radius:
                        cur += floor(q / (1 + d))
                if cur > max_v:
                    ans = [i, j]
                    max_v = cur
        return ans
```

#### Java

```java
class Solution {
    public int[] bestCoordinate(int[][] towers, int radius) {
        int[] ans = new int[2];
        int max = 0;
        for(int i = 0; i <= 50; i++) {
            for(int j = 0; j <= 50; j++) {
                int cur = 0;
                for(int[] v : towers) {
                    int x = v[0], y = v[1], q = v[2];
                    double d = Math.sqrt((x - i) * (x - i) + (y - j) * (y - j));
                    if(d <= radius) {
                        cur += (int)(q / (1 + d));
                    }
                }
                if(max < cur) {
                    ans[0] = i; ans[1] = j;
                    max = cur;
                }
            }
        }
        return ans;
    }
}
```

#### Go

```go
func bestCoordinate(towers [][]int, radius int) []int {
    ans := []int{0, 0}
    max := 0
    for i := 0; i <= 50; i++ {
        for j := 0; j <= 50; j++ {
            cur := 0
            for _, v := range towers {
                x, y, q := v[0], v[1], v[2]
                d := math.Sqrt(float64((x - i) * (x - i) + (y - j) * (y - j)))
                if d <= float64(radius) {
                    cur += int(float64(q) / (1 + d))
                }
            }
            if cur > max {
                max = cur
                ans[0] = i
                ans[1] = j
            }
        }
    }
    return ans
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第33篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~
