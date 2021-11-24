---
title: 【LeetCode】1095. Find in Mountain Array 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Hard
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpg
abbrlink: 24e5a86
date: 2021-08-08 22:31:39
---

# 问题描述

(This problem is an interactive problem.)

You may recall that an array A is a mountain array if and only if:

- A.length >= 3
- There exists some i with 0 < i < A.length - 1 such that:
    - A[0] < A[1] < ... A[i-1] < A[i]
    - A[i] > A[i+1] > ... > A[A.length - 1]

Given a mountain array mountainArr, return the minimum index such that mountainArr.get(index) == target.  If such an index doesn't exist, return -1.

You can't access the mountain array directly.  You may only access the array using a MountainArray interface:

- MountainArray.get(k) returns the element of the array at index k (0-indexed).

- MountainArray.length() returns the length of the array.

Submissions making more than 100 calls to MountainArray.get will be judged Wrong Answer.  Also, any solutions that attempt to circumvent the judge will result in disqualification.

## 测试样例

```
Input: array = [1,2,3,4,5,3,1], target = 3
Output: 2
Explanation: 3 exists in the array, at index=2 and index=5. Return the minimum index, which is 2.
```

```
Input: array = [0,1,2,4,2,1], target = 3
Output: -1
Explanation: 3 does not exist in the array, so we return -1.
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

如果可以找到峰值，就可以将问题简化为在有序数组中寻找目标值。

- 使用二分法，确定峰值位置

- 在左侧使用二分法寻找目标

- 若左侧未找到目标，在右侧使用二分法寻找目标

**补充：**

1. 时间复杂度 `O(logn)`

## 代码

```java
/**
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface MountainArray {
 *     public int get(int index) {}
 *     public int length() {}
 * }
 */
 
class Solution {
    public int findInMountainArray(int target, MountainArray mountainArr) {
        
        int pivot_idx = findPivotIdx(mountainArr);
        
        // 在左半部分寻找
        int idx = findInSortedArray(mountainArr, 0, pivot_idx, target, true);
        
        // 在右半部分寻找
        if(idx == -1) {
            idx = findInSortedArray(mountainArr, pivot_idx, mountainArr.length() - 1, target, false);
        }
        
        return idx;
    }
    
    /**
     * 寻找峰值坐标
     */
    public int findPivotIdx(MountainArray mountainArr) {
        int l = 0, r = mountainArr.length() - 1;
        
        // 当剩余 2 个元素时，退出循环
        // 其中值较大者为目标
        while(l + 1 < r) {
            int mid = (l + r) / 2;
            int value_m = mountainArr.get(mid);
            
            // pivot 在右边
            if(mid == l || value_m > mountainArr.get(mid - 1)) {
                l = mid;
            }
            // pivot 在左边
            else if(mid == r || value_m > mountainArr.get(mid + 1)) {
                r = mid;
            }
        }
        
        return mountainArr.get(l) > mountainArr.get(r) ? l : r;
    }
    
    /**
     * 在有序数组中寻找目标值
     */
    public int findInSortedArray(MountainArray mountainArr, int left, int right, int target, boolean ascent) {
        int l = left, r = right;
        
        while(l <= r) {
            int mid = (l + r) / 2;
            int value_m = mountainArr.get(mid);
            
            if(value_m == target) {
                return mid;
            }
            // 目标在左侧
            else if((ascent == true && value_m > target) || (ascent == false && value_m < target)) {
                r = mid - 1;
            }
            // 目标在右侧
            else {
                l = mid + 1;
            }
        }
        
        return -1;
    }
}
```


