---
title: LC Solution 1768. Merge Strings Alternately
date: 2022-10-23 15:05:56
tags: [算法]
categories: [Leetcode题解]
---

# [1768. 交替合并字符串](https://leetcode.cn/problems/merge-strings-alternately/)

## 题目描述

给你两个字符串 `word1` 和 `word2` 。请你从 `word1` 开始，通过交替添加字母来合并字符串。如果一个字符串比另一个字符串长，就将多出来的字母追加到合并后字符串的末尾。

返回 **合并后的字符串** 。

**示例 1：**

```
输入：word1 = "abc", word2 = "pqr"
输出："apbqcr"
解释：字符串合并情况如下所示：
word1：  a   b   c
word2：    p   q   r
合并后：  a p b q c r
```

**示例 2：**

```
输入：word1 = "ab", word2 = "pqrs"
输出："apbqrs"
解释：注意，word2 比 word1 长，"rs" 需要追加到合并后字符串的末尾。
word1：  a   b 
word2：    p   q   r   s
合并后：  a p b q   r   s
```

示例 3：

```
输入：word1 = "abcd", word2 = "pq"
输出："apbqcd"
解释：注意，word1 比 word2 长，"cd" 需要追加到合并后字符串的末尾。
word1：  a   b   c   d
word2：    p   q 
合并后：  a p b q c   d
```

## 解决方案

### 方法一：简单模拟

根据题意，从 `word1` 与 `word2` 中获取每一位字符，拼接到结果字符串中。

#### Python3

普通解法

```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        ans = []
        n = min(len(word1), len(word2))
        for i in range(n):
            ans.append(word1[i])
            ans.append(word2[i])
        ans = "".join(ans)
        return ans + word1[n:] if len(word1) > len(word2) else ans + word2[n:]
```

一行解法

```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        return "".join(a + b for a, b in zip_longest(word1, word2, fillvalue=""))
```

#### Java

```java
class Solution {
    public String mergeAlternately(String word1, String word2) {
        StringBuilder ans = new StringBuilder();
        int n1 = word1.length(), n2 = word2.length();
        for(int i = 0; i < n1 || i < n2; i++) {
            if(i < n1) ans.append(word1.charAt(i));
            if(i < n2) ans.append(word2.charAt(i));
        }
        return ans.toString();
    }
}
```

#### Go

```go
func mergeAlternately(word1 string, word2 string) string {
    ans := make([]byte, 0)
    n1, n2 := len(word1), len(word2)
    for i := 0; i < n1 || i < n2; i++ {
        if i < n1 {
            ans = append(ans, word1[i])
        }
        if i < n2 {
            ans = append(ans, word2[i])
        }
    }
    return string(ans)
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第30篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~