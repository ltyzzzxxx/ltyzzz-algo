---
title: LC-Solution 1249. Minimum Remove to Make Valid Parentheses
date: 2022-10-09 18:38:42
tags: [算法]
categories: [Leetcode题解]
---

# [1249. 移除无效的括号](https://leetcode.cn/problems/minimum-remove-to-make-valid-parentheses/)

## 题目描述

给你一个由 `'('`、`')'` 和小写字母组成的字符串 `s`。

你需要从字符串中删除最少数目的 `'('` 或者 `')'` （可以删除任意位置的括号)，使得剩下的「括号字符串」有效。

请返回任意一个合法字符串。

有效「括号字符串」应当符合以下 **任意一条** 要求：

- 空字符串或只包含小写字母的字符串
- 可以被写作 `AB`（`A` 连接 `B`）的字符串，其中 `A` 和 `B` 都是有效「括号字符串」
- 可以被写作 `(A)` 的字符串，其中 `A` 是一个有效的「括号字符串」

**示例1：**

```
输入：s = "lee(t(c)o)de)"
输出："lee(t(c)o)de"
解释："lee(t(co)de)" , "lee(t(c)ode)" 也是一个可行答案。
```

**示例2：**

```
输入：s = "a)b(c)d"
输出："ab(c)d"
```

**示例3：**

```
输入：s = "))(("
输出：""
解释：空字符串也是有效的
```

**提示：**

- `1 <= s.length <= 105`
- `s[i]` 可能是 `'('`、`')'` 或英文小写字母

## 解决方案

### 方法一：栈

一看到合法括号题，第一反应首先想到用栈解决。

题目要求删掉字符串 `s` 中的无效括号，使得剩余的所有括号构成有效括号，要求删除次数尽可能的少。

简单直接地理解题目：对 `s` 做减法，删除左右括号中数量较多的一方以达成有效括号

但是应思考如何尽可能地优雅且简单地从字符串中删除对应的字符

如果直接对字符串动手脚，下标会产生变化。

**思路转化：**

选择将字符串 `s` 转为数组，将需要删除的括号置为空即代表删除。

栈用于存储 `'('` 的下标，便于通过下标对字符串数组进行操作。

#### Python3

```python
class Solution:
    def minRemoveToMakeValid(self, s: str) -> str:
        stack, s = [], list(s)
        for i, x in enumerate(s):
            if x == '(':
                stack.append(i)
            elif x == ')':
                if stack:
                    stack.pop()
                else:
                    # 此时没有'('与')'配对，删除
                    s[i] = ''
        # 栈中剩余的均为'('
        for i in stack:
            # 此时没有')'与'('配对，删除
            s[i] = ''
        return ''.join(s)
```

#### Java

Java无法直接对字符赋空值，先填一个占位字符，之后利用replace方法将其替换为空。

```java
class Solution {
    public String minRemoveToMakeValid(String s) {
        char[] cs = s.toCharArray();
        Deque<Integer> stack = new ArrayDeque<>();
        for(int i = 0; i < cs.length; i++) {
            if(cs[i] == '(') stack.addLast(i);
            else if(cs[i] == ')') {
                if(!stack.isEmpty()) stack.pollLast();
                else cs[i] = '.';
            }
        }
        for(int i : stack) cs[i] = '.';
        return new String(cs).replace(".", "");
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第18篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~