---
title: 【LeetCode】162. Find Peak Element 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 46ffc75b
date: 2021-08-08 22:39:46
---

# 问题描述

A peak element is an element that is strictly greater than its neighbors.

Given an integer array nums, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.

You may imagine that nums[-1] = nums[n] = -∞.

You must write an algorithm that runs in O(log n) time.

## 测试样例

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

## 说明

```
Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
```

# 解题

## 思路

注意条件中的 `nums[i] != nums[i + 1]`，即相邻元素不同

再考虑到 `nums[-1] = nums[n] = -∞`

则整个数组必有一个峰值，其左右元素均比它小，不管存在多少个波峰，我们寻找该峰值（实际上为最大值）即可

**补充：**

1. 时间复杂度 `O(logn)`

## 代码

```java
class Solution {
    public int findPeakElement(int[] nums) {
        // 注意条件中的 nums[i] != nums[i + 1]，即相邻元素不同
        // 再考虑到 nums[-1] = nums[n] = -无穷
        // 则整个数组必有一个峰值，寻找该峰值即可
        int l = 0, r = nums.length - 1;
        
        while(l < r) {
            int mid = (l + r) / 2;
            
            int value_m_l = mid == 0 ? Integer.MIN_VALUE : nums[mid - 1];
            int value_m_r = mid == nums.length - 1 ? Integer.MAX_VALUE : nums[mid + 1];
            
            // peak value
            if(value_m_l < nums[mid] && nums[mid] > value_m_r) {
                return mid;
            }
            // 右侧值更大，区间缩小（包含最大值）
            else if(nums[mid] < value_m_r) {
                l = mid + 1;
            }
            // 左侧值更大，区间缩小（包含最大值）
            else {
                r = mid;
            }
        }
        
        return l;
    }
}
```


