---
title: LC-Solution 779. K-th Symbol in Grammar
date: 2022-10-20 13:45:27
tags: [算法]
categories: [Leetcode题解]
---

# [779. K-th Symbol in Grammar](https://leetcode.cn/problems/k-th-symbol-in-grammar/)

## 题目描述

我们构建了一个包含 `n` 行( **索引从 1 开始** )的表。首先在第一行我们写上一个 `0`。接下来的每一行，将前一行中的`0`替换为`01`，`1`替换为`10`。

-   例如，对于 `n = 3` ，第 `1` 行是 `0` ，第 `2` 行是 `01` ，第3行是 `0110` 。

给定行数 `n` 和序数 `k`，返回第 `n` 行中第 `k` 个字符。（ `k` **从索引 1 开始**）

**示例 1:**

```
输入: n = 1, k = 1
输出: 0
解释: 第一行：0
```

**示例 2:**

```
输入: n = 2, k = 1
输出: 0
解释: 
第一行: 0 
第二行: 01
```

**示例 3:**

```
输入: n = 2, k = 2
输出: 1
解释:
第一行: 0
第二行: 01
```

**提示:**

-   `1 <= n <= 30`
-   `1 <= k <= 2n - 1`

### 解决方案

#### 方法一：找规律 + 二分迭代

以n=5，k=10为例

```
n = 1 -> 0
n = 2 -> 01
n = 3 -> 0110
n = 4 -> 0110 1001
n = 5 -> 0110 1001 1001 0110
```

从中我们很容易发现，每一个 `n` 对应的长度为：`len = 2 ^ (n-1)`，用 `s` 表示每个 `n` 对应的字符串

因此我们发现这样一个规律：

-   如果 `k > len / 2`，那么 `s[k] = !s[k - len / 2]`。换句话说如果 `s[k]` 为1，那么 `s[k - len / 2]`为0，互为取反。

借助此规律，我们可以首先计算出对应的长度，用 `flag` 表示当前位置是否取反（很容易得知取反次数为偶数次时，保持不变）

循环迭代：

-   通过长度 `len` 判断是当前位置 `k` 是在左半边还是在右半边，以此决定是否取反。
    -   若 `k` 在右半边，则取反 `flag = !flag`，并将 `k -= len / 2`
-   每次对当前字符串进行二分，即长度 `len` 进行折半
-   每次对 `n` 进行自减

最终当 `n = 2` 时，停止迭代。此时 `k = 1`或 `k = 2`，便可根据 `flag` 得出最终答案

##### Python3

```python
class Solution:
    def kthGrammar(self, n: int, k: int) -> int:
        length = 2 ** (n - 1)
        if length <= 2:
            return k - 1
        flag = True
        while n > 2:
            if k > length // 2:
                k -= length // 2
                flag = not flag
            n -= 1
            length //= 2
        if flag:
            return 0 if k == 1 else 1
        else:
            return 1 if k == 1 else 0
```

##### Java

```java
class Solution {
    public int kthGrammar(int n, int k) {
        int len = (int)Math.pow(2, n - 1);
        if(len <= 2) return k - 1;
        boolean flag = true;
        while(n > 2) {
            if(k > len / 2) {
                k -= len / 2;
                flag = !flag;
            }
            n--;
            len /= 2;
        }
        if(flag) {
            return k == 1 ? 0 : 1;
        } else {
            return k == 1 ? 1 : 0;
        }
    }
}
```

##### Go

```go
func kthGrammar(n int, k int) int {
    len := int(math.Pow(2, float64(n - 1)))
    if len <= 2 {
        return k - 1
    }
    flag := true
    for n > 2 {
        if k > len / 2 {
            k -= len / 2
            flag = !flag
        }
        n--
        len /= 2
    }
    if flag {
        if k == 1 {
            return 0
        } 
        return 1
    } else {
        if k == 1 {
            return 1
        }
        return 0
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第28篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~