---
title: LC Solution 1668. Maximum Repeating Substring
date: 2022-11-03 13:23:39
tags: [算法]
categories: [Leetcode题解]
---

# [1668. 最大重复子字符串](https://leetcode.cn/problems/maximum-repeating-substring/)

## 题目描述

给你一个字符串 `sequence` ，如果字符串 `word` 连续重复 `k` 次形成的字符串是 `sequence` 的一个子字符串，那么单词 `word` 的 **重复值为 `k`** 。单词 `word` 的 **最****大重复值** 是单词 `word` 在 `sequence` 中最大的重复值。如果 `word` 不是 `sequence` 的子串，那么重复值 `k` 为 `0` 。

给你一个字符串 `sequence` 和 `word` ，请你返回 **最大重复值 `k`** 。

**示例 1：**

```
输入：sequence = "ababc", word = "ab"
输出：2
解释："abab" 是 "ababc" 的子字符串。
```

**示例 2：**

```
输入：sequence = "ababc", word = "ba"
输出：1
解释："ba" 是 "ababc" 的子字符串，但 "baba" 不是 "ababc" 的子字符串。
```

**示例 3：**

```
输入：sequence = "ababc", word = "ac"
输出：0
解释："ac" 不是 "ababc" 的子字符串。
```

**提示：**

-   `1 <= sequence.length <= 100`
-   `1 <= word.length <= 100`
-   `sequence` 和 `word` 都只包含小写英文字母。

## 解决方案

### 方法一：模拟

题目要求当 `word` 重复出现 `k` 次时形成的字符串为 `sequence` 的子字符串。要求返回最大的 `k` 值。

我们可以假设理想情况下，即 `word` 重复 `k` 次之后与 `sequence` 相同。

不难得出：理想情况下，当 `k` 最大时，应当满足 `ceil(len(word) * k) == len(sequence)`。其中 `ceil` 表示向上取整。

因此，我们可以先通过 `len(sequence) // len(word)` 求得初始理论上最大的 `k`，然后循环递减 `k`，直到 `word` 重复 `k` 次后形成的字符串为 `sequence` 的子字符串

#### Python3

```python3
class Solution:
    def maxRepeating(self, sequence: str, word: str) -> int:
        s_len, w_len = len(sequence), len(word)
        cnt = s_len // w_len
        while cnt:
            if sequence.find(word * cnt) != -1:
                return cnt
            cnt -= 1
        return 0
```

#### Java

```java
class Solution {
    public int maxRepeating(String sequence, String word) {
        int sLen = sequence.length(), wLen = word.length();
        int cnt = sLen / wLen;
        while(cnt > 0) {
            String repeatWord = repeatK(word, cnt);
            if(sequence.indexOf(repeatWord) != -1) {
                return cnt;
            }
            cnt--;
        }
        return 0;
    }

    public String repeatK(String word, int cnt) {
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < cnt; i++) {
            sb.append(word);
        }
        return sb.toString(); 
    }
}
```

#### Go

```go
func maxRepeating(sequence string, word string) int {
    sLen, wLen := len(sequence), len(word)
    cnt := sLen / wLen
    for cnt > 0 {
        repeatWord := repeatK(word, cnt)
        if strings.Contains(sequence, repeatWord) {
            return cnt
        }
        cnt--
    }
    return 0
}

func repeatK(word string, cnt int) (ans string) {
    for i := 0; i < cnt; i++ {
        ans += word
    }
    return
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第34篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~
