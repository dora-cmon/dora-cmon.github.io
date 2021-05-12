---
title: 【LeetCode】665. Non-decreasing Array 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 贪心
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: a460efb0
date: 2021-05-08 17:41:19
---


# 问题描述

Given an array nums with n integers, your task is to check if it could become non-decreasing by modifying at most one element.

We define an array is non-decreasing if nums[i] <= nums[i + 1] holds for every i (0-based) such that (0 <= i <= n - 2).

## 测试样例

```
input: nums = [4,2,3]
output: true
explanation: you could modify the first 4 to 1 to get a non-decreasing array.
```

```
Input: nums = [4,2,1]
Output: false
Explanation: You can't get a non-decreasing array by modify at most one element.
```

## 说明

```
n == nums.length
1 <= n <= 104
-105 <= nums[i] <= 105
```

# 解题

## 思路

找到非递增的项，有两种情况

1. 如 `2 4 3 5 7` 序列中的第二个元素 `4`， 其前一元素 `2` 小于后一元素 `3`，则将 `4` 降为 `3`，序列变成 `2 3 3 5 7`
1. 如 `2 4 1 5 7` 序列中的第二个元素 `4`， 其前一元素 `2` 大于后一元素 `1`，则将 `1` 升为 `4`，序列变成 `2 4 4 5 7`

已修改的元素及其之前的元素是非减序列，判断其余元素即可

若出现第二个非递增项，返回 false

**补充：**

1. 贪心算法
1. 时间复杂度 `O(nlogn)`（排序）

## 代码

```java
class Solution {
    public boolean checkPossibility(int[] nums) {
        // 是否已找到
        boolean flag_find = false;
        
        for(int i=0; i<nums.length-1; ++i) {
            // 找到非递增的项
            if(nums[i] > nums[i+1]) {
                // 第二次找到，直接返回
                if(flag_find)
                    return false;
                else {
                    flag_find = true;
                    // 若前一元素小于后一元素，降低当前元素，使其处于前后元素大小之间
                    if(i == 0 || nums[i - 1] <= nums[i + 1]) {
                        // 将第 i 个值降为 i + 1 的值
                        nums[i] = nums[i + 1];
                    }
                    // 若前一元素大于后一元素，提升后一元素，使其等于当前元素
                    else if(nums[i -1] > nums[i + 1]) {
                        // 将第 i + 1 个值升为 i 的值
                        nums[i + 1] = nums[i];
                    }
                    
                    // 判断其余元素是否符合非减序列
                    for(int j=i; j<nums.length-1; ++j) {
                        if(nums[j] > nums[j + 1])
                            return false;
                    }
                    
                }
            }
        }
        return true;
    }
}
```

