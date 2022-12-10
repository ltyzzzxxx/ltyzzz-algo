---
title: LC-Solution 901. Online Stock Span
date: 2022-10-21 12:33:44
tags: [算法]
categories: [Leetcode题解]
---

# [901. 股票价格跨度](https://leetcode.cn/problems/online-stock-span/)

## 题目描述

编写一个 `StockSpanner` 类，它收集某些股票的每日报价，并返回该股票当日价格的跨度。

今天股票价格的跨度被定义为股票价格小于或等于今天价格的最大连续日数（从今天开始往回数，包括今天）。

例如，如果未来7天股票的价格是 `[100, 80, 60, 70, 60, 75, 85]`，那么股票跨度将是 `[1, 1, 1, 2, 1, 4, 6]`。

**示例：**

```
输入：["StockSpanner","next","next","next","next","next","next","next"], [[],[100],[80],[60],[70],[60],[75],[85]]
输出：[null,1,1,1,2,1,4,6]
解释：
首先，初始化 S = StockSpanner()，然后：
S.next(100) 被调用并返回 1，
S.next(80) 被调用并返回 1，
S.next(60) 被调用并返回 1，
S.next(70) 被调用并返回 2，
S.next(60) 被调用并返回 1，
S.next(75) 被调用并返回 4，
S.next(85) 被调用并返回 6。

注意 (例如) S.next(75) 返回 4，因为截至今天的最后 4 个价格
(包括今天的价格 75) 小于或等于今天的价格。
```

**提示：**

1.  调用 `StockSpanner.next(int price)` 时，将有 `1 <= price <= 10^5`。
2.  每个测试用例最多可以调用 `10000` 次 `StockSpanner.next`。
3.  在所有测试用例中，最多调用 `150000` 次 `StockSpanner.next`。
4.  此问题的总时间限制减少了 50%。

## 解决方案

### 方法一：单调栈

题目要求我们返回比不超过当前股票价格的最大连续天数：进一步分析可知，我们只要找到之前第一个大于当前股票价格的下标，两个天数对应的下标相减，便是答案。

因此我们可以运用单调栈，按照股票的价格以降序入栈。

每个单调栈中存放形如 `[prices, days]`这样的数组：

-   prices：当前股票的价格
-   days：股票价格的跨度（即在此之前，不超过当前股票价格的最大连续天数）

next方法被调用时，我们便进行入栈操作。在入栈之前，我们需要循环判断当前栈顶元素的 `price` 是否小于等于当前股票的 `price`:

-   若小于等于，则对栈顶元素进行出栈，并且将栈顶元素的 `days` 累加到当前股票的 `days` 上

循环判断完毕后，当前股票的 `price` 为最小股票价格，满足单调递减，即可入栈。并将当前股票对应的 `days` 返回。

#### Python3

```python3
class StockSpanner:
    def __init__(self):
        self.stack = []

    def next(self, price: int) -> int:
        stack = self.stack
        days = 1 # days要初始化为1，因为最大连续天数中包含其自身
        while stack and stack[-1][0] <= price:
            days += stack[-1][1]
            stack.pop(-1)
        stack.append([price, days])
        return days
```

#### Java

```java
class StockSpanner {
    Deque<int[]> d;
    public StockSpanner() {
        d = new ArrayDeque<>();
    }
    
    public int next(int price) {
        int days = 1; // days要初始化为1，因为最大连续天数中包含其自身
        while (!d.isEmpty() && d.peekLast()[0] <= price) {
            days += d.pollLast()[1];
        }
        int[] arr = new int[]{price, days};
        d.addLast(arr);
        return days;
    }
}
```

#### Go

```go
type StockSpanner struct {
    stack [][2] int
}

func Constructor() StockSpanner {
    return StockSpanner{[][2] int {}}
}

func (this *StockSpanner) Next(price int) int {
    days := 1 // days要初始化为1，因为最大连续天数中包含其自身
    for len(this.stack) > 0 && price >= this.stack[len(this.stack) - 1][0] {
        days += this.stack[len(this.stack) - 1][1]
        this.stack = this.stack[:len(this.stack) - 1]
    }
    this.stack = append(this.stack, [2]int{price, days})
    return days
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第29篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~