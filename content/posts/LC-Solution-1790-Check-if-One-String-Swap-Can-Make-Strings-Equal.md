---
title: LC-Solution 1790. Check if One String Swap Can Make Strings Equal
date: 2022-10-11 12:25:55
tags: [算法]
categories: [Leetcode题解]
---

# [1790. 仅执行一次字符串交换能否使两个字符串相等](https://leetcode.cn/problems/check-if-one-string-swap-can-make-strings-equal/)

## 题目描述

给你长度相等的两个字符串 `s1` 和 `s2` 。一次 **字符串交换** 操作的步骤如下：选出某个字符串中的两个下标（不必不同），并交换这两个下标所对应的字符。

如果对 **其中一个字符串** 执行 **最多一次字符串交换** 就可以使两个字符串相等，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：s1 = "bank", s2 = "kanb"
输出：true
解释：例如，交换 s2 中的第一个和最后一个字符可以得到 "bank"
```

**示例 2：**

```
输入：s1 = "attack", s2 = "defend"
输出：false
解释：一次字符串交换无法使两个字符串相等
```

**示例 3：**

```
输入：s1 = "kelb", s2 = "kelb"
输出：true
解释：两个字符串已经相等，所以不需要进行字符串交换
```

**示例 4：**

```
输入：s1 = "abcd", s2 = "dcba"
输出：false
```

**提示**

- `1 <= s1.length, s2.length <= 100`
- `s1.length == s2.length`
- `s1` 和 `s2` 仅由小写英文字母组成

## 解决方案

### 方法一：哈希表 + 计数

思路：

- 先用哈希表统计字符串 `s1` 与 `s2` 的词频，只有词频相同的情况下，才可能满足题目要求的交换一次便得到相同字符串。
  
- 计数：遍历，对两个字符串逐个判断，得到不同字符数量 `cnt` 。
  
  - 若 `s1[i] != s2[i]`，`cnt += 1`
- 最终，当 `cnt == 0` 或 `cnt == 2`，分别对应着交换次数为0和交换次数为1的情况。（前提是两字符串词频相同）
  

该方法缺点在于空间复杂度为O(n)

#### Python3

```python
class Solution:
    def areAlmostEqual(self, s1: str, s2: str) -> bool:
        dic1, dic2 = Counter(s1), Counter(s2)
        cnt = 0
        for i in range(len(s1)):
            if s1[i] != s2[i]:
                cnt += 1
        return (cnt == 0 or cnt == 2) and dic1 == dic2
```

#### java

```java
class Solution {
    public boolean areAlmostEqual(String s1, String s2) {
        char[] cs1 = s1.toCharArray(), cs2 = s2.toCharArray();
        Map<Character, Integer> map1 = new HashMap<>(), map2 = new HashMap<>();
        int cnt = 0;
        for(int i = 0; i < cs1.length; i++) {
            map1.put(cs1[i], map1.getOrDefault(cs1[i], 0) + 1);
            map2.put(cs2[i], map2.getOrDefault(cs2[i], 0) + 1);
            if(cs1[i] != cs2[i]) cnt++;
        }
        return (cnt == 0 || cnt == 2) && map1.equals(map2);
    }
}
```

### 方法二：纯计数

思路：

仍然是同时遍历两字符串 `s1` 与 `s2`，用 `cnt` 统计相同下标不同字符的数量。

但不采用哈希表统计词频，而是用两个变量 `c1` 与 `c2` 记录上一次相同下标的情况下 `s1` 与 `s2` 不相同的字符。

若 `cnt == 2`时，当前 `s1[i] != c2` 或 `s2[i] != c1`，此时可能出现如下情况

- `bank` 与 `canb`，隐含的意思就是两字符串词频不同，只不过这个方法咱们没有用哈希表进行统计。
  
  因为题目中说只可交换一次，所以只需判断 `cnt == 2` 这种情况下词频是否相同。
  

最终遍历完成之后，还需要判断 cnt 是否等于 1，防止出现如下情况

- `bank` 与 `cank`，只有一个下标的字符不相同，隐含意思是词频不相同，无法交换。

#### Python3

```python
class Solution:
    def areAlmostEqual(self, s1: str, s2: str) -> bool:
        cnt = 0
        c1, c2 = '', ''
        for i in range(len(s1)):
            if s1[i] != s2[i]:
                cnt += 1
                if cnt > 2 or (cnt == 2 and (s1[i] != c2 or s2[i] != c1)):
                    return False
                c1, c2 = s1[i], s2[i]
        return cnt != 1
```

#### Java

```java
class Solution {
    public boolean areAlmostEqual(String s1, String s2) {
        char[] cs1 = s1.toCharArray(), cs2 = s2.toCharArray();
        int cnt = 0;
        char c1 = 0, c2 = 0;
        for(int i = 0; i < cs1.length; i++) {
            if(cs1[i] != cs2[i]) {
                if(++cnt > 2 || (cnt == 2 && (cs1[i] != c2 || cs2[i] != c1))) {
                    return false;
                }
                c1 = cs1[i];
                c2 = cs2[i];
            }
        }
        return cnt != 1;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第19篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~