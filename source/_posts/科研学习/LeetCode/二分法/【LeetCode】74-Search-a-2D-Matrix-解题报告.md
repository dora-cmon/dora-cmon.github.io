---
title: 【LeetCode】74. Search a 2D Matrix 解题报告
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 90d1a8b3
date: 2021-08-08 22:47:55
---

# 问题描述

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.

## 测试样例

![](/images/【LeetCode】74-Search-a-2D-Matrix-解题报告/2021-08-08-22-48-45.png)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
```

![](/images/【LeetCode】74-Search-a-2D-Matrix-解题报告/2021-08-08-22-49-36.png)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
```

## 说明

```
m == matrix.length
n == matrix[i].length
1 <= m, n <= 100
-10^4 <= matrix[i][j], target <= 10^4
```

# 解题

## 思路

首先使用二分法判断目标所在行，然后再使用二分法判断该行中目标所在位置。

**补充：**

1. 时间复杂度 `O(logn)`

## 代码

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row = locateRaw(matrix, target);
        
        int l = 0, r = matrix[0].length - 1;
        
        while(l <= r) {
            int mid = l + (r - l) / 2;
            
            if(matrix[row][mid] == target) {
                return true;
            }
            // 在左侧
            else if(matrix[row][mid] > target) {
                r = mid - 1;
            }
            // 在右侧
            else {
                l = mid + 1;
            }
        }
        
        return false;
    }
    
    /**
     * 定位目标所在行
     */
    public int locateRaw(int[][] matrix, int target) {
        int top = 0, bottom = matrix.length - 1;
        
        while(top < bottom) {
            int mid = top + (bottom - top) / 2;
            
            // 在 mid 行中
            if(matrix[mid][0] <= target && target <= matrix[mid][matrix[mid].length - 1]) {
                return mid;
            }
            // 在 mid 行之下
            else if(matrix[mid][0] < target) {
                top = mid + 1;
            }
            // 在 mid 行之上
            else {
                bottom = mid - 1;
            }
        }
        return top;
    }
}
```
