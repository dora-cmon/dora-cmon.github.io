---
title: 【LeetCode】4. Median of Two Sorted Arrays 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Hard
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 1f34f97d
date: 2021-05-31 10:40:45
---

# 问题描述

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).

## 测试样例

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

```
Input: nums1 = [0,0], nums2 = [0,0]
Output: 0.00000
```

```
Input: nums1 = [], nums2 = [1]
Output: 1.00000
```

```
Input: nums1 = [2], nums2 = []
Output: 2.00000
```

## 说明

```
nums1.length == m
nums2.length == n
0 <= m <= 1000
0 <= n <= 1000
1 <= m + n <= 2000
-10^6 <= nums1[i], nums2[i] <= 10^6
```

# 解题

## 思路

将问题一般化为：寻找两个有序数组中的第 `K` 个数，`K` 为两数组长度和的一半。

整体思路为，将数组 1 和 2 分别切分为前后两部分，总共为 4 部分，每次过滤掉最小的一部分，并在 `k` 中减去对应元素数。切分依据为，每次选取数组 1 和 2 的第 `k/2` 位，若数组 1 中元素不足，则取 1 中全部元素，并在 2 中补齐。（在递归中，通过判断保证 `nums1` 剩余元素始终小于 `nums2`）

递归有两个出口：

- 当数组 1 中元素已全部被掠过，即 `beg1` 已经到达 `nums1` 的长度，直接返回数组 2 中的第剩余 `k` 位

- 当 `k` 被减到 `1`，则代表下一个元素即为目标，则 `nums1[beg1]` 和 `nums2[beg2]` 中的较小者为目标

**补充：**

1. 二分法
2. 时间复杂度 `O(lg(m+n))`

## 代码

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        // 一般化为寻找两个有序数组中的第 K 个数
        int k = (nums1.length + nums2.length + 1) / 2;

        if((nums1.length + nums2.length) % 2 == 0) {    // 偶数个元素
            return (findKthSortedArrays(nums1, 0, nums2, 0, k) + findKthSortedArrays(nums1, 0, nums2, 0, k + 1)) / 2.0;
        } else {                                        // 奇数个元素
            return (double)findKthSortedArrays(nums1, 0, nums2, 0, k);
        }
    }

    /*
    * 寻找两个有序数组中的第 K 个数
    */
    public int findKthSortedArrays(int[] nums1, int beg1, int[] nums2, int beg2, int k) {

        if(nums1.length - beg1 > nums2.length - beg2) {             // 保证 nums1 子数组长度较小
            return findKthSortedArrays(nums2, beg2, nums1, beg1, k);
        }
        // 由于 nums1 子数组长度较小，若两数组剩余元素不足 k/2，必然为 nums1
        if(beg1 == nums1.length) {          // 若 nums1 数组元素已经全部被排除
            return nums2[beg2 + k - 1];         // 返回 nums2 剩余 k 对应位
        }
        if(k == 1) {                        // 若下一个元素即为目标，则为 nums1[beg1] 和 nums2[beg2] 中的较小者
            return Math.min(nums1[beg1], nums2[beg2]);
        }
        
        // 偏置量，以 k/2 为基准，数组1 若不足则取剩余全部，数组2 补齐至 k
        int bias1 = Math.min(nums1.length - beg1, k / 2), bias2 = k - bias1;
        // 将偏置转换为数组下标
        int pos1 = beg1 + bias1 - 1, pos2 = beg2 + bias2 - 1;

        // 将数组 1 和 2 分别切分为前后两部分，总共为 4 部分，每次过滤掉最小的一部分
        if(nums1[pos1] < nums2[pos2]) {
            return findKthSortedArrays(nums1, pos1 + 1, nums2, beg2, k - bias1);
        } else {
            return findKthSortedArrays(nums1, beg1, nums2, pos2 + 1, k - bias2);
        }
    }
}
```

