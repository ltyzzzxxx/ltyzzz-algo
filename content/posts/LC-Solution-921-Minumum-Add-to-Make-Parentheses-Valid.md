---
title: LC-Solution 921. Minumum Add to Make Parentheses Valid
date: 2022-10-04 13:44:32
tags: [算法]
categories: [Leetcode题解]
---

# [921. 使括号有效的最少添加](https://leetcode.cn/problems/minimum-add-to-make-parentheses-valid/)

## 题目描述

只有满足下面几点之一，括号字符串才是有效的：

- 它是一个空字符串，或者
  
- 它可以被写成 `AB` （`A` 与 `B` 连接）, 其中 `A` 和 `B` 都是有效字符串，或者
  
- 它可以被写作 `(A)`，其中 `A` 是有效字符串。
  

给定一个括号字符串 `s` ，移动N次，你就可以在字符串的任何位置插入一个括号。

例如，如果 `s = "()))"` ，你可以插入一个开始括号为 `"(()))"` 或结束括号为 `"())))"` 。
返回 为使结果字符串 `s` 有效而必须添加的最少括号数。

**示例1：**

```
输入：s = "())"
输出：1
```

**示例2：**

```
输入：s = "((("
输出：3
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 只包含 `'('` 和 `')'` 字符。

## 解决方案

### 方法一：栈

对于成对出现的括号题，合法的每一对括号是具有顺序的，可以直观地想到用栈解决。

将成对的括号移除出栈，最后栈中保留的一定是无法成对的括号数量，最终栈的大小即为需要添加的括号次数。

遍历字符串 `s`：

- 若 `s[i] == '('`，直接入栈。
  
- 若 `s[i] == ')'`，且当前栈不为空 and 栈顶元素为 `'('`，说明此时构成了一对合法括号，可以将栈顶的 `'('`出栈。若不满足此条件，则将 `s[i]` 入栈
  

#### Python3

```python
class Solution:
    def minAddToMakeValid(self, s: str) -> int:
        q = deque() 
        for x in s:
            if x == '(':
                q.append(x)
            else:
                if len(q) > 0 and q[-1] == '(':
                    q.pop()
                else:
                    q.append(x)
        return len(q)
```

#### Java

```java
class Solution {
    public int minAddToMakeValid(String s) {
        Deque<Character> d = new ArrayDeque<>();
        for(char x : s.toCharArray()) {
            if(x == '(') {
                d.addLast(x);
            } else {
                if(!d.isEmpty() && d.peekLast() == '(') {
                    d.pollLast();
                } else {
                    d.addLast(x);
                }
            }
        }
        return d.size();
    }
}
```

### 方法二：模拟计数

根据题意进行模拟，用 `cnt` 表示 `'('`的个数。

遍历字符串 `s`

- 若 `s[i] == '('`，cnt++
  
- 若 `s[i] == ')'`，cnt--，相当于消除一个 `'('`
  
  - 若此时进行 `cnt--` 操作之后，cnt变为-1，说明 `'('`数量不足，需要添加一个 `'('`进行额外配对，并将 `cnt` 重新置为0
    

#### Python3

```python
class Solution:
    def minAddToMakeValid(self, s: str) -> int:
        cnt, ans = 0, 0
        for x in s:
            if x == '(':
                cnt += 1
            else:
                cnt -= 1
            if cnt == -1:
                cnt = 0
                ans += 1
        return ans + cnt
```

#### Java

```java
class Solution {
    public int minAddToMakeValid(String s) {
        int cnt = 0, ans = 0;
        for(char x : s.toCharArray()) {
            cnt += x == '(' ? 1 : -1;
            if(cnt == -1) {
                cnt = 0;
                ans++;
            }
        }
        return ans + cnt;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第12篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~