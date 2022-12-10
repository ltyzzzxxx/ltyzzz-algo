---
title: LC Solution 784. Letter Case Permutation
date: 2022-10-30 14:53:16
tags: [算法]
categories: [Leetcode题解]
---

# [784. 字母大小写全排列](https://leetcode.cn/problems/letter-case-permutation/)

## 题目描述

给定一个字符串 `s` ，通过将字符串 `s` 中的每个字母转变大小写，我们可以获得一个新的字符串。

返回 *所有可能得到的字符串集合* 。以 **任意顺序** 返回输出。

**示例 1：**

```
输入：s = "a1b2"
输出：["a1b2", "a1B2", "A1b2", "A1B2"]
```

**示例 2:**

```
输入: s = "3z4"
输出: ["3z4","3Z4"]
```

**提示:**

-   `1 <= s.length <= 12`
-   `s` 由小写英文字母、大写英文字母和数字组成

## 解决方案

### 方法一：回溯

遇到全排列问题时，第一反应便是回溯解决。

题目要求遇到字母时，将其转换为大写或者小写字母，从而得到新的字符串。

从中可以想到，可以构造一个二叉树，二叉树的深度便是字符串 ` s` 中字母的数量。每个父节点是当前的字母，其两个子节点对应下一个字母的大小写。

构造回溯方法，其包含三个参数，第一个参数为题目给定的字符串 `s`， 第二个参数为当前正在构造的字符串 `cur`，第三个参数记录下标位置。遍历到的字符若为数字，则跳到下一位置；若为字母，则分别对字母的大小写进行递归遍历。

#### Python3

```python
class Solution:
    def letterCasePermutation(self, s: str) -> List[str]:
        def traverse(s, cur, begin):
            if len(cur) == len(s):
                ans.append(cur)
                return
            if s[begin].isalpha():
                traverse(s, cur + s[begin].swapcase(), begin + 1)
            traverse(s, cur + s[begin], begin + 1)
        ans = []
        traverse(s, '', 0)
        return ans
```

#### Java

```java
class Solution {
    List<String> ans;

    public List<String> letterCasePermutation(String s) {
        ans = new ArrayList<>();
        traverse(s, "", 0);
        return ans;
    }

    void traverse(String s, String cur, int begin) {
        if(s.length() == cur.length()) {
            ans.add(cur);
            return;
        }
        traverse(s, cur + s.charAt(begin), begin + 1);
        if(Character.isLetter(s.charAt(begin))) {
            traverse(s, cur + (char)(s.charAt(begin) ^ 32), begin + 1);
        }
    }   
}
```

#### Go

```go
func letterCasePermutation(s string) []string {
    ans := make([]string, 0)
    var f func (string, string, int)
    f = func(s string, cur string, begin int) {
        if len(s) == len(cur) {
            ans = append(ans, cur)
            return
        }
        f(s, cur + string(s[begin]), begin + 1)
        if unicode.IsLetter(rune(s[begin])) {
            f(s, cur + string(s[begin] ^ 32), begin + 1);
        }
    }
    f(s, "", 0)
    return ans
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第32篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~