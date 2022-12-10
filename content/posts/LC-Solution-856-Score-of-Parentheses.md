---
title: LC-Solution 856. Score of Parentheses
date: 2022-10-09 14:13:29
tags: [算法]
categories: [Leetcode题解]
---

# [856. Score of Parentheses](https://leetcode.cn/problems/score-of-parentheses/)

## 题目描述

给定一个平衡括号字符串 `S`，按下述规则计算该字符串的分数：

- `()` 得 1 分。
- `AB` 得 `A + B` 分，其中 A 和 B 是平衡括号字符串。
- `(A)` 得 `2 * A` 分，其中 A 是平衡括号字符串。

**示例1：**

```
输入： "()"
输出： 1
```

**示例2：**

```
输入： "(())"
输出： 2
```

**示例3：**

```
输入： "()()"
输出： 2
```

**示例4：**

```
输入： "(()(()))"
输出： 6
```

**提示：**

1. `S` 是平衡括号字符串，且只含有 `(` 和 `)` 。
2. `2 <= S.length <= 50`

## 解决方案

### 方法一：根据()计数

分析题意可知，外层括号所累加的分数都与最里括号 `'()'` 有关。

- 假设当前括号深度为 `d`，每一个 `'()'` 的得分为 `2 ** d` 或者 `1 << d`
  
- 最后的总分数与 `'()'` 的数量有关。
  

举例说明：`'(()(()))'`，利用乘法分配律可表示为 `'(()) + ((()))'`

共有两个 `'()'`，其分数为 `2 ** 1 + 2 ** 2 = 6`

所以，我们只需要找到 `s` 中的每一个 `'()'` 即可

#### Python3

```python
class Solution:
    def scoreOfParentheses(self, s: str) -> int:
        ans, d = 0, 0
        for i, v in enumerate(s):
            d += 1 if v == '(' else -1
            if v == ')' and s[i - 1] =='(':
                ans += 1 << d
        return ans
```

#### Java

```java
class Solution {
    public int scoreOfParentheses(String s) {
        int ans = 0, d = 0;
        for(int i = 0; i < s.length(); i++) {
            d += s.charAt(i) == '(' ? 1 : -1;
            if(s.charAt(i) == ')' && s.charAt(i - 1) == '(') {
                ans += 1 << d;
            }
        }
        return ans;
    }
}
```

### 方法二：栈

采用栈记录字符串 `s` 对应的分数。初始栈顶部的分数为0。

遍历字符串：

1. 若 `s[i] == '('`，先将分数0压入栈顶占位
  
2. 若 `s[i] == ')'`，此时有两种情况：
  
  - `s[i - 1] == '('`，这是一个 `'()'`， 此时栈顶值 `v` 为0，该分数为1
    
  - `s[i - 1] != '('`，这是一个外层括号，该分数为栈顶值 `v` × 2
    
  
  两种情况综合为 `max(v * 2, 1)`
  
  将栈顶值出栈计算完成之后，再将计算结果累加到当前的栈顶值。
  

最终返回结果为栈顶值。

这是因为一开始还未遍历字符串 `s` 时，就已经初始化了栈顶值。而且该字符串 `s` 为平衡括号字符串，说明 `'(' `与 `')'` 数量一致，所以最后栈中一个值，即为最终分数。

#### Python3

```python
class Solution:
    def scoreOfParentheses(self, s: str) -> int:
        stack = [0]
        for x in s:
            if x == '(':
                stack.append(0)
            else:
                v = stack.pop()
                stack[-1] += max(v * 2, 1)
        return stack[0]
```

#### Java

```java
class Solution {
    public int scoreOfParentheses(String s) {
        Deque<Integer> stack = new ArrayDeque<>();
        stack.addLast(0);
        for(char c : s.toCharArray()) {
            if(c == '(') stack.addLast(0);
            else {
                int v = stack.pollLast();
                stack.addLast(stack.pollLast() + Math.max(v * 2, 1));
            }
        }
        return stack.pollLast();
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第16篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~