---
title: LC-Solution 926. Flip String to Monotone Increasing
date: 2022-10-03 21:11:31
tags: [算法]
categories: [Leetcode题解]
---

# [926. Flip String to Monotone Increasing](https://leetcode.cn/problems/flip-string-to-monotone-increasing/)

## 题目描述

如果一个二进制字符串，是以一些 `0`（可能没有 `0`）后面跟着一些 `1`（也可能没有 `1`）的形式组成的，那么该字符串是 **单调递增** 的。

给你一个二进制字符串 `s`，你可以将任何 `0` 翻转为 `1` 或者将 `1` 翻转为 `0` 。

返回使 `s` 单调递增的最小翻转次数。

**示例1：**

```
输入：s = "00110"
输出：1
解释：翻转最后一位得到 00111.
```

**示例2：**

```
输入：s = "010110"
输出：2
解释：翻转得到 011111，或者是 000111。
```

**示例3：**

```
输入：s = "00011000"
输出：2
解释：翻转得到 00000000。
```

**提示：**

- `1 <= s.length <= 105`
- `s[i]` 为 `'0'` 或 `'1'`

## 解决方案

### 方法一：前缀和

题目要求通过翻转 `0` 与 `1` 使得最后得到的二进制字符串为单调递增，

通过此可以想到：统计字符 `s[i]` 左边 `1` 数目，右边 `0` 的数目。将这些 `1` 与 `0` 翻转，最终的字符串 `s` 即可满足单调递增，翻转次数 = 左边 `1` 的数目 + 右边 `0` 的数目

因此，该问题可以采用前缀和的思路解决。

- 设置 `sum` 数组统计前缀和（长度为 `len(s) + 1` ）
  
- `sum[i]` 代表字符 `s[i]` 左边 `1` 的数目（不包含 `i` ）
  
- 通过 `sum[i]` 可计算得到 `s[i]` 右边 `0` 的数目（包含 `i`）
  
  - 右边 `0` 的数目为：`(len(s) - i) - (sum[len(s)] - sum[i])`

遍历两次字符串 `s` ，求得最终答案

- 第一次遍历：求得前缀和数组 `sum`
  
- 第二次遍历：计算当前翻转次数（左边 `1` 的数目 + 右边 `0` 的数目）并与 `ans` 比较，取最小值。
  

#### Python3

```python
class Solution:
    def minFlipsMonoIncr(self, s: str) -> int:
        n, ans = len(s), len(s)
        sum = [0] * (n + 1) 
        for i in range(1, n + 1):
            sum[i] = sum[i - 1] + (s[i - 1] == '1')
        for i in range(0, n + 1):
            l_ones, r_zeros = sum[i], n - i - (sum[n] - sum[i])
            ans = min(ans, l_ones + r_zeros)
        return ans
```

#### Java

```java
class Solution {
    public int minFlipsMonoIncr(String s) {
        int n = s.length();
        char[] cs = s.toCharArray();
        int[] sum = new int[n + 1];
        int ans = n;
        for(int i = 1; i <= n; i++) {
            sum[i] = sum[i - 1] + (cs[i - 1] == '1' ? 1 : 0);
        }
        for(int i = 0; i <= n; i++) {
            int leftOnes = sum[i], rightZeros = n - i - (sum[n] - sum[i]);
            ans = Math.min(ans, leftOnes + rightZeros);
        }
        return ans;
    }
}
```

### 方法二：优化前缀和

方法一种提到：

- `s[i]` 左边 `1` 的数目为 `sum[i]`
  
- `s[i]` 右边 `0` 的数目为 `(len(s) - i) - (sum[len(s)] - sum[i])`
  
- 遍历一轮 `s`，二者求和的最小值即为答案
  

即 `left_ones + right_zeros = 2 * sum[i] - i + len(s) - sum[len(s)]`

因此，只需保证 `2 * sum[i] - i` 为最小值，然后求得所有 `1` 的数目，即可得到最终的翻转次数，采用常数级空间复杂度即可实现。

注意：根据之前的定义，这里 `i` 的最大值仍然为 `len(s)`

#### Python3

```python
class Solution:
    def minFlipsMonoIncr(self, s: str) -> int:
        n, ans, l_ones = len(s), inf, 0
        for i in range(n + 1):
            ans = min(ans, 2 * l_ones - i)
            # 防止越界
            if i < n:
                l_ones += s[i] == '1'
        return ans + n - l_ones;
```

#### Java

```java
class Solution {
    public int minFlipsMonoIncr(String s) {
        int n = s.length(), ans = Integer.MAX_VALUE, leftOnes = 0;
        for(int i = 0; i <= n; i++) {
            ans = Math.min(ans, 2 * leftOnes - i);
            if(i < n) {
                leftOnes += s.charAt(i) == '1' ? 1 : 0;
            }
        }
        return ans + n - leftOnes;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第11篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~