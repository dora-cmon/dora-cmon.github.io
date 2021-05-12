---
title: 【LeetCode】435. Non-overlapping Intervals 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 贪心
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 1ab92857
date: 2021-04-23 17:54:22
---

# 问题描述

Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

## 测试样例

```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```

```
Input: intervals = [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.
```

```
Input: intervals = [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

## 说明

```
1 <= intervals.length <= 2 * 104
intervals[i].length == 2
-2 * 104 <= starti < endi <= 2 * 104
```

# 解题

## 思路

选择的区间结尾越小，余留给其它区间的空间就越大，就越能保留更多的区间。

因此，我们采取的贪心策略为，优先保留结尾小且不相交的区间。


**补充：**

1. 贪心算法
1. 数组自定义排序
    ```java
    Arrays.sort(array, new Comparator<T>() {
        @Override
        public int compare(T a, T b) {
            // 排序策略
            return a - b;
        }
    })
    ```
1. 时间复杂度 `O(nlogn)`（排序）

## 代码

```java
import java.util.Arrays;
import java.util.Comparator;

class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {

        int count = 0;

        // 按区间尾元素大小排序
        Arrays.sort(intervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] a, int[] b) {
                return a[1] - b[1];
            }
        });

        // 已占用区间尾
        int tail_idx = Integer.MIN_VALUE;
        // 若区间被占用，则移除该区间
        for(int i=0; i<intervals.length; ++i) {
            // 可放入
            if(intervals[i][0] >= tail_idx) {
                tail_idx = intervals[i][1];
            } else {
                ++count;
            }
        }
        
        return count;
    }
}
```
