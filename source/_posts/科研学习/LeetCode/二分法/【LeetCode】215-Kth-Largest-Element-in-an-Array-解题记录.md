---
title: 【LeetCode】215. Kth Largest Element in an Array 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpg
abbrlink: 9a60758
date: 2021-08-08 22:09:54
---

# 问题描述

Given an integer array nums and an integer k, return the kth largest element in the array.

Note that it is the kth largest element in the sorted order, not the kth distinct element.

## 测试样例

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

## 说明

```
1 <= k <= nums.length <= 10^4
-10^4 <= nums[i] <= 10^4
```

# 解题

## 思路

寻找第 `K` 大的数即为寻找递增序的第 `length - K` 个数。

使用二分法的思想来解决，那么如何确定基准？

根据快排的思想，每一步确定一个元素的位置，并使其左侧元素值不大于该元素，右侧元素值不小于该元素。以该元素作为基准。

 - 若目标在基准点右侧，则取右半部分，并更新 k

 - 若目标在基准点左侧，则取左半部分


**补充：**

1. 时间复杂度 `O(logn)`

## 代码

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        
        // 转为坐标
        int k_remain = nums.length - k;
        
        int l = 0, r = nums.length - 1;
        while(true) {
            // 基准点坐标
            int div_idx = partition(nums, l, r);
            // 基准点相对 l 的位置
            int div_seq = div_idx - l;
            
            // k 在基准点右侧，更新 k
            if(div_seq < k_remain) {
                l = div_idx + 1;
                k_remain -= ++div_seq;
            }
            // k 在基准点左侧
            else if(div_seq > k_remain) {
                r = div_idx - 1;
            }
            // k 恰好为基准点
            else {
                break;
            }
        }
        
        return nums[l + k_remain];
    }
    
    /**
     * 取区间首元素作为基准，划分数组
     * return 基准坐标
     */
    public int partition(int[] nums, int l, int r) {
        int pivot = nums[l];
        
        while(l < r) {
            while(l < r && nums[r] >= pivot) {
                --r;
            }
            nums[l] = nums[r];
            
            while(l < r && nums[l] < pivot) {
                ++l;
            }
            nums[r] = nums[l];
        }
        
        nums[l] = pivot;
        
        return l;
    }
}
```


