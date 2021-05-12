---
title: 【LeetCode】452. Minimum Number of Arrows to Burst Balloons 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 贪心
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 71cefafc
date: 2021-05-07 16:55:35
---

# 问题描述

There are some spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter, and hence the x-coordinates of start and end of the diameter suffice. The start is always smaller than the end.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps traveling up infinitely.

Given an array points where points[i] = [xstart, xend], return the minimum number of arrows that must be shot to burst all balloons.

## 测试样例

```
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).
```

```
Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4
```

```
Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2
```

## 说明

```
1 <= points.length <= 104
points[i].length == 2
-231 <= xstart < xend <= 231 - 1
```

# 解题

## 思路

区间重叠情况，一般可以使用贪心算法。

把所有的区间按照右边界进行排序。遍历时只要向当前气球的右边界射箭，就能打破尽可能多的气球。然后依次移动射箭的位置，进行统计即可。

**补充：**

1. 先排序
1. 贪心算法
1. 时间复杂度 `O(nlogn)`（排序）

## 代码

```java
import java.util.Arrays;

class Solution {
    public int findMinArrowShots(int[][] points) {
        
        // 按区间右侧数字，从小到大排序
        Arrays.sort(points, (arr1, arr2) -> arr1[1] - arr2[1]);
        
        // arrow 数量
        int cnt_arrow = 1;
        // arrow 位置
        int p_arrow = points[0][1];
        // 遍历气球
        for(int i=0; i<points.length; ++i) {
            // 第 i 个气球的左右边界
            int p_left = points[i][0], p_right = points[i][1];
            
            // arrow 坐标在当前气球边界外
            if(p_arrow < p_left || p_right < p_arrow) {
                p_arrow = p_right;  // 将 arrow 坐标设为当前气球右侧边界
                ++cnt_arrow;        // arrow 数量增加
            } // arrow 坐标在当前气球边界内，直接下一个
            
        }
        
        return cnt_arrow;
    }
}
```