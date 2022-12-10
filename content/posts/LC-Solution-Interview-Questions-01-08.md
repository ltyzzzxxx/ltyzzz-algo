---
title: LC-Solution Interview Questions 01.08
date: 2022-10-03 20:51:54
tags: [算法]
categories: [Leetcode题解]
---

# [01.08. Zero Matrix LCCI](https://leetcode.cn/problems/zero-matrix-lcci/)

## 题目描述

编写一种算法，若M × N矩阵中某个元素为0，则将其所在的行与列清零。

**示例1：**

```
输入：
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
输出：
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

**示例2**

```
输入：
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
输出：
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

## 解决方案

### 方法一：模拟

题目要求当二维数组中的某个元素值为0时，将其所在的行和列的全部元素置为0。

通过分析题意，可以想到采用标记数组存储符合题意的行与列。

- 第一次遍历：标记行与列
  
- 第二次遍历：对标记的行与列中的元素进行修改
  

#### Python3

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        row = [False] * len(matrix)
        col = [False] * len(matrix[0])
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j] == 0:
                    row[i] = col[j] = True
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                matrix[i][j] = 0 if row[i] == True or col[j] == True else matrix[i][j]
```

#### Java

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean[] row = new boolean[m], col = new boolean[n];
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(matrix[i][j] == 0) {
                    row[i] = col[j] = true;
                }
            }
        }
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(row[i] == true || col[j] == true) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

## Encouragement

目前就读于新加坡国立大学，转码菜鸡一枚。

决心通过日更一篇题解激励自己坚持刷题，坚持学习算法。

这是我搭建[Leetcode-Everyday](https://github.com/ltyzzzxxx/Leetcode-Everyday)仓库后发布の第3篇题解，欢迎各位star并提出Issue~

点击[这里](https://leetcode.cn/u/ltyzzz/)，进入我的个人Leetcode主页~