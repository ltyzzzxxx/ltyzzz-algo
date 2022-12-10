---
title: LC-Solution 3. Longest Substring Without Repeating Characters
date: 2022-10-03 20:45:58
tags: [算法]
categories: [Leetcode题解]
---

# [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

## 题目描述

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例1：**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例2：**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例3：**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

**提示：**

- `0 <= s.length <= 5 * 104`
- `s` 由英文字母、数字、符号和空格组成

## 解决方案：

### 方法一：滑动窗口

题目要求从给定字符串`s`中找出不包含重复字符的最长子串的长度

由于子串是连续的，可以想到采用滑动窗口解决该问题。

思路如下：

- 采用双指针`i`与`j`作为滑动窗口的左端点与右端点，初始值为0
  
- 采用哈希表记录已经遍历过的字符。其`key`为字符本身，`value`为当前字符对应的下标
  
- 设`maxLen`为不包含重复字符的最长子串的长度
  
- 从头到尾遍历字符串`s`
  
  1. 判断哈希表是否存储当前字符`c`
    
    - 若哈希表已存储当前字符`c`
      
      - 需要收缩滑动窗口，即更新滑动窗口左端点`i`的位置
        
        - 此时，左端点`i`有两种选择，第一种为保持`i`不变，第二种为将其更新为哈希表中当前字符`c`对应的下标再加1
          
          1. 如果当前滑动窗口不包含当前字符`c`但哈希表中却存储着`c`，则需要选择第一种更新方案。例如`abba`字符串。
            
          2. 如果当前滑动窗口包含当前字符`c`，则需要选择第二种更新方案。例如`abca`字符串。
            
        
        . 为综合以上两种更新方案，左端点`i`更新时区二者最大值即可。
        
  2. 更新哈希表
    
  3. 比较当前滑动窗口长度与`maxLen`的大小，并为`maxLen`赋值
    
  4. 滑动窗口向右滑动，`j`自增
    

#### Python3

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s:
            return 0
        dic = defaultdict(int)
        maxlen, left = 0, 0
        for i in range(len(s)):
            if s[i] in dic:
                left = max(dic[s[i]] + 1, left)
            dic[s[i]] = i
            maxlen = max(maxlen, i - left + 1)
        return maxlen
```

#### Java

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s.length() == 0) return 0;
        int left = 0, n = s.length();
        int maxLen = 0, curLen = 0;
        Set<Character> set = new HashSet<>();
        for(int i = 0; i < n; i++) {
            curLen++;
            while(set.contains(s.charAt(i))) {
                set.remove(s.charAt(left));
                left++;
                curLen--;
            }
            maxLen = Math.max(curLen, maxLen);
            set.add(s.charAt(i));
        }
        return maxLen;
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第6篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~