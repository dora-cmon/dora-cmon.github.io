---
title: 【LeetCode】33. Search in Rotated Sorted Array 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 84c602fe
date: 2021-08-08 22:23:32
---

# 问题描述

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is rotated at an unknown pivot index k (0 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

## 测试样例

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

```
Input: nums = [1], target = 0
Output: -1
```

## 说明

```
1 <= nums.length <= 5000
-10^4 <= nums[i] <= 10^4
All values of nums are unique.
nums is guaranteed to be rotated at some pivot.
-10^4 <= target <= 10^4
```

# 解题

## 思路

使用二分法，重点在于如何确定该向哪一侧递归。

判断 `nums[left]` 和 `nums[mid]` 的相对大小：

- 若 `left` 值大于 `mid` 值，则前半部分非递增，后半部分有序

    - 根据目标是否在有序一侧，缩小范围（`nums[mid] <= target && target <= nums[r]`）

- 若 left 值小于 mid 值，则后半部分非递增，前半部分有序

    - 根据目标是否在有序一侧，缩小范围（`nums[l] <= target && target <= nums[mid]`）

- 若两值相等，则无法判断哪侧有序

    - 令左边界右移一位（`left` 值和 `mid` 值相等，舍弃 `left` 值还有 `mid` 值，不影响结果）

**补充：**

1. 时间复杂度 `O(logn)`

## 代码

```java
class Solution {
    public int search(int[] nums, int target) {
        
        int l = 0, r = nums.length - 1;
        
        while(l <= r) {
            int mid = (l + r) / 2;
            
            if(nums[mid] == target) {
                return mid;
            }
            
            // 前半部分非递增，后半部分有序
            if(nums[mid] < nums[l]) {
                // 若目标在有序部分
                if(nums[mid] <= target && target <= nums[r]) {
                    l = mid + 1;
                } else {
                    r = mid -1;
                }
            }
            // 后半部分非递增，前半部分有序
            else if(nums[mid] > nums[l]) {
                // 若目标在有序部分
                if(nums[l] <= target && target <= nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            }
            // 无法判断哪侧有序
            else {
                ++l;
            }
        }
        
        // 未找到
        return -1;
    }
}
```


