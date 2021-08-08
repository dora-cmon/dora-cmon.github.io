---
title: 【LeetCode】56. Merge Intervals 解题报告
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 排序
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 7cb44be5
date: 2021-08-08 21:55:24
---

# 问题描述

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

## 测试样例

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

## 说明

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

# 解题

## 思路

首先，将数组按照 `start` 排序，遍历数组：

- 若当前元素为第一个或 `start >= 最新 interval 的 end`，加入结果

- 若 `start < 最新 interval 的 end`，且 `end > 最新 interval 的 end`，调整最新 `interval` 的 `end` 为当前元素的 `end`

**补充：**

1. 时间复杂度 `O(logn)`
1. 注意判断被最新 `interval` 完全包裹的元素

## 代码

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        List<int[]> results = new ArrayList<int[]>();
        
        // 按照 start 排序
        Arrays.sort(intervals, (a, b) -> a[0] - b[0] );
        
        for(int[] pair : intervals) {
            if(results.size() == 0 || pair[0] > results.get(results.size() - 1)[1]) {
                // 若为 首个 或 start >= 已有 interval 的 end，加入结果
                results.add(pair);
            }
            else if(pair[1] > results.get(results.size() - 1)[1]) {
                // 若 start < 已有 interval 的 end，
                // 且 end > 已有 interval 的 end，
                // 调整已有 insterval 的 end 为 新的 end
                results.get(results.size() - 1)[1] = pair[1];
            }
        }
        
        return results.toArray(new int[results.size()][]);
    }
}
```


