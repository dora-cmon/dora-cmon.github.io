---
title: 【LeetCode】154. Find Minimum in Rotated Sorted Array II 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Hard
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: dc05f3e5
date: 2021-05-27 19:37:53
---

# 问题描述

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,4,4,5,6,7] might become:

[4,5,6,7,0,1,4] if it was rotated 4 times.
[0,1,4,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums that may contain duplicates, return the minimum element of this array.

You must decrease the overall operation steps as much as possible.

## 测试样例

```
Input: nums = [1,3,5]
Output: 1
```

```
Input: nums = [2,2,2,0,1]
Output: 0
```

## 说明

```
n == nums.length
1 <= n <= 5000
-5000 <= nums[i] <= 5000
nums is sorted and rotated between 1 and n times.
```

# 解题

## 思路

由于数组旋转一次，则从最小值所在的位开始，数组无序，且这个节点之前或之后的子数组都是有序的。

使用二分法，判断中点前后是否有序来决定在哪一侧寻找结果：

- 若中点小于最右侧点，则右侧必有序，在左侧寻找（可能中点为目标）
- 若中点大于最右侧点，则右侧必无序，在右侧寻找
- 若中点等于最右侧点，无法判断哪侧有序，右指针左移（最右侧点和中点相同，保留一个即可，右侧点可略过）

**判断中点及最右侧点**，若数组未旋转，最终会找到首位；若判断左侧点和中点则会找到末尾。

**补充：**

1. 二分法
2. 时间复杂度 `O(lg(n))`

## 代码

```java
class Solution {
    public int findMin(int[] nums) {
        
        int left = 0, right = nums.length - 1;
        
        while(left < right) {
            int mid = (left + right) / 2;
            
            if(nums[mid] == nums[right]) {          // 无法判断哪侧有序
                --right;                                // 右指针左移，该值在 mid 位重复，可略过
            } else if(nums[mid] < nums[right]) {    // 右侧有序
                right = mid;                            // 在左侧寻找，可能为 mid 位
            } else {                                // 右侧无序
                left = mid + 1;                         // 在右侧寻找
            }
        }
        
        return nums[left];
    }
}
```