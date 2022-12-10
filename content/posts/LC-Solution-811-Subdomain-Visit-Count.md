---
title: LC-Solution 811. Subdomain Visit Count
date: 2022-10-05 14:01:07
tags: [算法]
categories: [Leetcode题解]
---

# [811. 子域名访问计数](https://leetcode.cn/problems/subdomain-visit-count/)

## 题目描述

网站域名 `"discuss.leetcode.com"` 由多个子域名组成。顶级域名为 `"com"` ，二级域名为 `"leetcode.com"` ，最低一级为 `"discuss.leetcode.com"` 。当访问域名 `"discuss.leetcode.com"` 时，同时也会隐式访问其父域名 `"leetcode.com"` 以及 `"com"` 。

**计数配对域名** 是遵循 `"rep d1.d2.d3"` 或 `"rep d1.d2"` 格式的一个域名表示，其中 `rep` 表示访问域名的次数，`d1.d2.d3` 为域名本身。

例如，`"9001 discuss.leetcode.com"` 就是一个 **计数配对域名** ，表示 `discuss.leetcode.com` 被访问了 `9001` 次。
给你一个 **计数配对域名** 组成的数组 `cpdomains` ，解析得到输入中每个子域名对应的 **计数配对域名** ，并以数组形式返回。可以按 **任意顺序** 返回答案。

**示例1：**

```
输入：cpdomains = ["9001 discuss.leetcode.com"]
输出：["9001 leetcode.com","9001 discuss.leetcode.com","9001 com"]
解释：例子中仅包含一个网站域名："discuss.leetcode.com"。
按照前文描述，子域名 "leetcode.com" 和 "com" 都会被访问，所以它们都被访问了 9001 次。
```

**示例2：**

```
输入：cpdomains = ["900 google.mail.com", "50 yahoo.com", "1 intel.mail.com", "5 wiki.org"]
输出：["901 mail.com","50 yahoo.com","900 google.mail.com","5 wiki.org","5 org","1 intel.mail.com","951 com"]
解释：按照前文描述，会访问 "google.mail.com" 900 次，"yahoo.com" 50 次，"intel.mail.com" 1 次，"wiki.org" 5 次。
而对于父域名，会访问 "mail.com" 900 + 1 = 901 次，"com" 900 + 50 + 1 = 951 次，和 "org" 5 次。
```

提示：

- `1 <= cpdomain.length <= 100`
  
- `1 <= cpdomain[i].length <= 100`
  
- `cpdomain[i]` 会遵循 `"repi d1i.d2i.d3i"` 或 `"repi d1i.d2i"` 格式
  
- `repi` 是范围 `[1, 104]` 内的一个整数
  
- `d1i`、`d2i` 和 `d3i` 由小写英文字母组成
  

## 解决方案

### 方法一：哈希表

通过哈希表统计 `cpdomains` 中所有的子域名的频率。

以 `google.mail.com` 为例，`google.mail.com` 为三级域名，`mail.com` 为二级域名，`com`为顶级域名。

思路如下：

1. 遍历 `cpdomains` 列表，获取每一个 `cpdomain` 的访问次数 `cnt` 与其域名 `domain`
  
2. 将 `domain` 的访问次数累加至原哈希表
  
3. 继续遍历 `domain`的子域名字符串
  
  1. 若 `domain[i] == '.'`，说明其之后为一个完整的子域名，将其访问次数累加到哈希表中。
4. 遍历结束之后，将哈希表的 `K` 与 `V` 以列表的形式返回
  

#### Python3

```python
class Solution:
    def subdomainVisits(self, cpdomains: List[str]) -> List[str]:
        dic = defaultdict(int)
        for d in cpdomains:
            ds = d.split()
            cnt, domain = int(ds[0]), ds[1]
            dic[domain] += cnt
            for i, x in enumerate(domain):
                if x == '.':
                    dic[domain[i+1:]] += cnt
        return [f"{v} {k}" for k, v in dic.items()]
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第13篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~