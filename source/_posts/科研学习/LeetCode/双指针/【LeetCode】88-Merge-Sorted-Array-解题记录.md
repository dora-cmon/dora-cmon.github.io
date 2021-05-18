---
title: 【LeetCode】88. Merge Sorted Array 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 双指针
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 9648cc77
date: 2021-05-12 13:55:46
---

# 问题描述

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

The number of elements initialized in nums1 and nums2 are m and n respectively. You may assume that nums1 has a size equal to m + n such that it has enough space to hold additional elements from nums2.

## 测试样例

```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
```

```
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
```

## 说明

```
nums1.length == m + n
nums2.length == n
0 <= m, n <= 200
1 <= m + n <= 200
-109 <= nums1[i], nums2[i] <= 109
```

# 解题

## 思路

数组已经排好序，可以采用双指针来求解。

- `p` 指针指向 `nums1` 的末尾，
- `p1, p2` 两个指针分别指向两数组的末尾。
- 每次将较大的数字复制到 `nums1` 的 `p` 位置，然后指针向前移动一位。

结束后，若 `nums2` 有剩余，则将剩余元素添加到正确位置；若 `nums1` 有剩余，因为结果基于 `nums1` 构建，因此无需操作剩余元素即在正确位置

**补充：**

1. 双指针
1. 时间复杂度 `O(m + n)`

## 代码

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // 插入位置指针
        int p = m + n - 1;        
        // 当前指针，分别指向 nums1 数组和 nums2 数组
        int p1 = m - 1, p2 = n - 1;
        // 从后向前遍历
        while(p1 >= 0 && p2 >= 0) {
            // 将 nums1 和 nums2 中当前位置元素较大的，添加到正确位置，并移动指针
            nums1[p--] = nums1[p1] > nums2[p2] ? nums1[p1--] : nums2[p2--];
        }
        // 若 nums2 有剩余，添加到正确位置，
        // 若 nums1 有剩余，无需操作即在正确位置
        while(p2 >= 0) {
            nums1[p--] = nums2[p2--];
        }
    }
}
```
