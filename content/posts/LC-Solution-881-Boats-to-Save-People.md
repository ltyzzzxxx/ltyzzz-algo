---
title: LC-Solution 881. Boats to Save People
date: 2022-10-03 21:10:04
tags: [算法]
categories: [Leetcode题解]
---

# [881. 救生艇](https://leetcode.cn/problems/boats-to-save-people/)

## 题目描述

给定数组 `people` 。`people[i]`表示第 `i` 个人的体重 ，**船的数量不限**，每艘船可以承载的最大重量为 `limit`。

每艘船最多可同时载两人，但条件是这些人的重量之和最多为 `limit`。

返回 *承载所有人所需的最小船数* 。

**示例1：**

```
输入：people = [1,2], limit = 3
输出：1
解释：1 艘船载 (1, 2)
```

**示例2：**

```
输入：people = [3,2,2,1], limit = 3
输出：3
解释：3 艘船分别载 (1, 2), (2) 和 (3)
```

**示例3：**

```
输入：people = [3,5,3,4], limit = 5
输出：4
解释：4 艘船分别载 (3), (3), (4), (5)
```

**提示：**

- `1 <= people.length <= 5 * 104`
- `1 <= people[i] <= limit <= 3 * 104`

## 解决方案

### 方法一：贪心 + 双指针

通过分析题意可以得出以下几点关键的信息：

1. 可用船数不限制
  
2. 每艘船限乘2人，每搜船限制总重为`limit`
  
3. 返回结果与`people`数组的顺序无关
  

为使得使用的船数最少，每搜船上的两个人的总重量最好 **等于或尽可能接近** `limit`

因此，不难得出本题的思路：对`people`数组进行升序排序，双指针遍历`people`数组。

需要注意的一点：双指针`i`与`j`重合时，此时只剩余一个人，所以他单独乘一艘船。

#### Python3

```python
class Solution:
    def numRescueBoats(self, people: List[int], limit: int) -> int:
        people.sort()
        i, j, ans = 0, len(people) - 1, 0
        while i <= j:
            if i == j:
                ans += 1
                break
            if people[i] + people[j] <= limit:
                ans, i, j = ans + 1, i + 1, j - 1
            else:
                ans, j = ans + 1, j - 1
        return ans
```

#### Java

```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int i = 0, j = people.length - 1;
        int ans = 0;
        while(i <= j) {
            if(i == j) {
                ans++;
                break;
            }
            if(people[i] + people[j] <= limit) {
                ans++; i++; j--;
            } else {
                ans++; j--;
            }
        }
        return ans;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第10篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~