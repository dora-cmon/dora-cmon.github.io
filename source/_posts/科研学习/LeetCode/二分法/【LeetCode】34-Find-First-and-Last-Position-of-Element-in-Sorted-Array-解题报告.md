---
title: 【LeetCode】34. Find First and Last Position of Element in Sorted Array 解题报告
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 39bf3160
date: 2021-08-08 22:19:00
---

# 问题描述

Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

## 测试样例

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

```
Input: nums = [], target = 0
Output: [-1,-1]
```

## 说明

```
0 <= nums.length <= 10^5
-10^9 <= nums[i] <= 10^9
nums is a non-decreasing array.
-10^9 <= target <= 10^9
```

# 解题

## 思路

根据二分法实现寻找数组中某元素上下界的函数即可，注意处理边界条件。

**补充：**

1. 时间复杂度 `O(logn)`

## 代码

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int beg = lower_bound(nums, target);
        int end = upper_bound(nums, target);
        
        return beg == end ? new int[]{-1, -1} : new int[]{beg, end - 1};
    }
    
    /**
     * 寻找下界，[beg, end)
     */
    public int lower_bound(int[] nums, int target) {
        int l = 0, r = nums.length;
        
        while(l < r) {
            int mid = (l + r) / 2;
            
            if(nums[mid] < target) {
                l = mid + 1;
            }
            else {
                r = mid;
            }
        }
        
        return l;
    }
    
    /**
     * 寻找上界，[beg, end)
     */
    public int upper_bound(int[] nums, int target) {
        int l = 0, r = nums.length;
        
        while(l < r) {
            int mid = (l + r) / 2;
            
            if(nums[mid] <= target) {
                l = mid + 1;
            }
            else {
                r = mid;
            }
        }
        
        return l;
    }
}
```


