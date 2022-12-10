---
title: LC-Solution 5. Longest Palindromic Substring
date: 2022-10-14 22:58:04
tags: [算法]
categories: [Leetcode题解]
---

# [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

## 题目描述

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

**示例 1：**

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例 2**

```
输入：s = "cbbd"
输出："bb"
```

**提示：**

-   `1 <= s.length <= 1000`
-   `s` 仅由数字和英文字母组成

## 解决方案

### 方法一：中心扩散法 + 双指针

思路为：从 `s` 中选取一个位置作为中心，从该中心位置向两边扩散，如果两边元素不相等的时候停止。

举个🌰：如 `abbac`，假设选取第一个'b'作为中心位置，`i = 2`。设左指针 `l == i - 1` ，右指针`r == i + 1` 

1.   先向左扩散。`l` 一直自减，直到遇到与中心位置不相等的字符。此时 `s[l] = 'a'` `l = 1` 
2.   再向右扩散。`r` 一直自增，直到遇到与中心位置不相等的字符。此时 `s[r] = 'a'` `r = 3`
3.   左指针与右指针同时开始双向扩散，直到左和右不相等

一开始之所以要执行第1步和第2步，就是要进一步确定中心块。因为定义的中心可能只有一个元素，但可能由多个相同元素组成。

遍历 `s`， 执行完上述流程后，需要更新最长回文子串的左端点与长度，以便于最后返回结果

#### Python3

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        maxLen, maxLeft = 0, 0
        for i in range(n):
            l, r = i - 1, i + 1
            curLen = 1
            while l >= 0 and s[l] == s[i]:
                curLen, l = curLen + 1, l - 1
            while r < n and s[r] == s[i]:
                curLen, r = curLen + 1, r + 1
            while l >= 0 and r < n and s[l] == s[r]:
                curLen += 2
                l, r = l - 1, r + 1
            if curLen >= maxLen:
                maxLen = curLen
                maxLeft = l
        return s[maxLeft + 1: maxLeft + maxLen + 1]
```

#### Java

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        char[] cs = s.toCharArray();
        int maxLen = 0, maxLeft = 0;
        for(int i = 0; i < cs.length; i++) {
            int l = i - 1, r = i + 1;
            int curLen = 1;
            while(l >= 0 && cs[l] == cs[i]) {
                curLen++;
                l--;
            }
            while(r < n && cs[r] == cs[i]) {
                curLen++;
                r++;
            }
            while(l >= 0 && r < n && cs[l] == cs[r]) {
                l--;
                r++;
                curLen += 2;
            }
            if(curLen > maxLen) {
                maxLen = curLen;
                maxLeft = l;
            }
        }
        return s.substring(maxLeft + 1, maxLeft + 1 + maxLen);
    }
}
```

### 方法二：动态规划

中心扩散法做了很多重复的计算。其实可以在计算的过程中将这些值存储起来，即记忆化搜索。可以采用动态规划实现。

设 `dp[i][j]`，其为`True` 代表 `[i, j]` 区间字符串为回文字符串

#### Python3

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        maxLen, maxLeft, maxRight = 1, 0, 0
        dp = [[False]*n for _ in range(n)]
        for r in range(1, n):
            for l in range(r):
                if s[l] == s[r] and (r - l <= 2 or dp[l+1][r-1]):
                    dp[l][r] = True
                    if r - l + 1 > maxLen:
                        maxLen = r - l + 1
                        maxLeft, maxRight = l, r
        return s[maxLeft: maxRight + 1]
```

#### Java

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        char[] cs = s.toCharArray();
        int maxLen = 0, maxLeft = 0, maxRight = 0;
        boolean[][] dp = new boolean[n][n];
        for(int r = 1; r < n; r++) {
            for(int l = 0; l < r; l++) {
                if(cs[l] == cs[r] && (r - l <= 2 || dp[l + 1][r - 1])) {
                    dp[l][r] = true;
                    if(r - l + 1> maxLen) {
                        maxLen = r - l + 1;
                        maxLeft = l;
                        maxRight = r;
                    }
                }
            }
        }
        return s.substring(maxLeft, maxRight + 1);
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第24篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~