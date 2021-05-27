---
title: 【LeetCode】34. Find First and Last Position of Element in Sorted Array 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: e40a1822
date: 2021-05-19 16:03:02
---

# 问题描述

Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

## 测试样例

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

实现 C++ 里的 `lower_bound` 和 `upper_bound` 函数，

分别使用二分法，寻找 `target` 的边界，

- 寻找左边界时，值 `>= target` 时，向左寻找，最终，`right` 指针必 `< target`，而 `left` 指针 `= right + 1` 且 `>= target` 
- 寻找右边界时，值 `<= target` 时，向右寻找，最终，`left` 指针必 `> target`，而 `right` 指针 `= left - 1` 且 `<= target`

**补充：**

1. 二分法
2. 时间复杂度 `O(lg(n))`

## 代码

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        // 首指针
        int p_head = lower_bound(nums, target);
        int p_tail = -1;     // 若存在再计算
        if(p_head < nums.length && nums[p_head] == target) {    // 存在 target
            p_tail = upper_bound(nums, target);
        } else {                        // 不存在 target
            p_head = -1;
        }
        
        return new int[]{p_head, p_tail};
    }
    
    /*
       寻找下边界
     */
    int lower_bound(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        // 二分
        while(left <= right) {
            int mid = (left + right) / 2;
            int val = nums[mid];
            
            if(val >= target) {     // 大了或相等，取左
                right = mid - 1;
            } else {                // 小了，取右
                left = mid + 1;
            }
        }
        // 最终 left 值 >= target， right 值 < target
        return left;
    }
    
    /*
       寻找下边界
     */
    int upper_bound(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        // 二分
        while(left <= right) {
            int mid = (left + right) / 2;
            int val = nums[mid];
            if(val <= target) {     // 小了或相等，取右
                left = mid + 1;
            } else{                 // 大了，取左
                right = mid - 1;
            }  
        }
        // 最终 right 值 <= target， left 值 > target
        return right;
    }
}
```
