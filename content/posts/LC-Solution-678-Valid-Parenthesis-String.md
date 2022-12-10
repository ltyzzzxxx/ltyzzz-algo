---
title: LC-Solution 678. Valid Parenthesis String
date: 2022-10-09 17:05:07
tags: [算法]
categories: [Leetcode题解]
---

# [678. 有效的括号字符串](https://leetcode.cn/problems/valid-parenthesis-string/)

## 题目描述

给定一个只包含三种字符的字符串：`（` ，`）` 和 `*`，写一个函数来检验这个字符串是否为有效字符串。有效字符串具有如下规则：

1. 任何左括号 `(` 必须有相应的右括号 `)`。
2. 任何右括号 `)` 必须有相应的左括号 `(` 。
3. 左括号 `(` 必须在对应的右括号之前 `)`。
4. `*` 可以被视为单个右括号 `)` ，或单个左括号 `(` ，或一个空字符串。
5. 一个空字符串也被视为有效字符串。

**示例1：**

```
输入: "()"
输出: True
```

**示例2：**

```
输入: "(*)"
输出: True
```

**示例3：**

```
输入: "(*))"
输出: True
```

**注意:**

1. 字符串大小将在 [1，100] 范围内。

## 解决方案

### 方法一：双栈

遇到括号题，第一反应便是栈。因为栈的先进后出的特性与括号之间嵌套的特性相一致。

栈内总是存储左括号 `'('`，通过右括号 `')'` 与左括号 `'('` 配对，使左括号出栈。

本题思路：

`'*'` 可以表示三种字符：`''` 、 `'('` 、`')'`，优先将 `'*'` 当作左括号使用。

因此使用两个栈分别存储 `'*'` 与 `'('` 对应于 `s` 的下标。

- 存储下标的意义在于：由于我们是优先将 `'*'` 当作左括号使用，当右括号 `')'` 全部消耗完。这时候多余的 `'*'` 将要作为右括号 `')'` 与剩余的 `'('` 进行配对。右括号必须在左括号之后。因此存储下标可以帮助我们判断 `'*'` 与 `'('` 的先后顺序是否符合合法括号的要求。
  
  `'('` 使用优先级 **大于** `'*'`
  

1. 遍历字符串 `s` ：
  
  . 若 `s[i] == '('`， 将其添加到 `'('` 栈中
  
  . 若 `s[i] == '*'`， 将其添加到 `'*'` 栈中
  
  . 若 `s[i] == '*'`：
  
  . `'('` 栈 或 `'*'` 栈不为空时，优先使用 `'('` 栈进行出栈
  
  . 否则，没有字符与 `'('` 进行配对，返回 `False`
  
2. 遍历 `'('` 与 `'*'` 栈，将 `'*'` 当作 `')'` 与 剩余 `'('` 进行匹配
  
  . 若弹出的 `'*'` 下标 **小于** 弹出的 `'('`， 返回False
  
3. 最终判断是否仍然有剩余的 `'('`
  
  . 若仍然有，返回False；否则，返回True
  

#### Python3

```python
class Solution:
    def checkValidString(self, s: str) -> bool:
        stack1, stack2 = [], []
        for i, x in enumerate(s):
            if x == '(':
                stack1.append(i)
            if x == '*':
                stack2.append(i)
            if x == ')':
                if stack1:
                    stack1.pop()
                elif stack2:
                    stack2.pop()
                else:
                    return False
        while stack1 and stack2:
            idx1, idx2 = stack1.pop(), stack2.pop()
            if idx1 > idx2:
                return False
        return not stack1
```

#### Java

```java
class Solution {
    public boolean checkValidString(String s) {
        Deque<Integer> stack1 = new ArrayDeque<>();
        Deque<Integer> stack2 = new ArrayDeque<>();
        char[] cs = s.toCharArray();
        for(int i = 0; i < cs.length; i++) {
            if(cs[i] == '(') stack1.addLast(i);
            else if(cs[i] == '*') stack2.addLast(i);
            else {
                if(!stack1.isEmpty()) stack1.pollLast();
                else if(!stack2.isEmpty()) stack2.pollLast();
                else return false;
            }
        }
        while(!stack1.isEmpty() && !stack2.isEmpty()) {
            int idx1 = stack1.pollLast(), idx2 = stack2.pollLast();
            if(idx1 > idx2) return false;
        }
        return stack1.isEmpty();
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第17篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~