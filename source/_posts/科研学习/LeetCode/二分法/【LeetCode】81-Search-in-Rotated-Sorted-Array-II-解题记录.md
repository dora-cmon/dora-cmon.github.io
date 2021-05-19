---
title: 【LeetCode】81. Search in Rotated Sorted Array II 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpeg
date: 2021-05-19 16:37:03
---

# 问题描述

There is an integer array nums sorted in non-decreasing order (not necessarily with distinct values).

Before being passed to your function, nums is rotated at an unknown pivot index k (0 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,4,4,5,6,6,7] might be rotated at pivot index 5 and become [4,5,6,6,7,0,1,2,4,4].

Given the array nums after the rotation and an integer target, return true if target is in nums, or false if it is not in nums.

## 测试样例

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

## 说明

```
1 <= nums.length <= 5000
-10^4 <= nums[i] <= 10^4
nums is guaranteed to be rotated at some pivot.
-10^4 <= target <= 10^4
```

# 解题

## 思路

即使数组被旋转过，我们仍然可以利用这个数组的递增性，使用二分查找。

对于当前的中点，
- 如果它指向的值小于等于右端，那么说明右区间是排好序的；反之，那么说明左区间是排好序的。
- 如果目标值位于排好序的区间内，我们可以对这个区间继续二分查找；反之，我们对于另一半区间继续二分查找。

注意，因为数组存在重复数字，如果中点和左端的数字相同，我们并不能确定是左区间全部相同，还是右区间完全相同。在这种情况下，我们可以简单地将左端点右移一位，然后继续进行二分查找。


**补充：**

1. 二分法
2. 时间复杂度 `O(lg(n))`

## 代码

```java
class Solution {
    public boolean search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while(left <= right) {
            
            int mid = (left + right) / 2;
            // 找到，返回
            if(nums[mid] == target) {         
                return true;
            }
            
            if(nums[left] < nums[mid]) {                        // 左边有序
                if(nums[left] <= target && target < nums[mid]) {     // target 在左区间中 [内)
                    right = mid - 1;
                } else {                                            // target 在右区间中 [)外
                    left = mid + 1;
                }
            } else if(nums[left] > nums[mid]) {                 // 右边有序
                if(nums[mid] < target && target <= nums[right]) {    // target 在右区间中 (内]
                    left = mid + 1;
                } else {                                            // target 在左区间中 外(]
                    right = mid - 1;
                }
            } else if(nums[left] == nums[mid]) {                // 无法判断哪边有序，target != nums[mid] -> target != nums[left]
                ++left;                                             // 右移左指针，结果不为左边界，可右移
            }
        }
        // 未找到
        return false;
    }
}
```
