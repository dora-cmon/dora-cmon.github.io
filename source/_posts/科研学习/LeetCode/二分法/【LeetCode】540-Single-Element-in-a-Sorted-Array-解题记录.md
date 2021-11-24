---
title: 【LeetCode】540. Single Element in a Sorted Array 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpg
abbrlink: d78f4506
date: 2021-05-27 16:01:37
---

# 问题描述

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once. Find this single element that appears only once.

Follow up: Your solution should run in O(log n) time and O(1) space.

## 测试样例

```
Input: nums = [1,1,2,3,3,4,4,8,8]
Output: 2
```

```
Input: nums = [3,3,7,7,10,11,11]
Output: 10
```

## 说明

```
1 <= nums.length <= 10^5
0 <= nums[i] <= 10^5
```

# 解题

## 思路

使用二分法，根据中点前后的元素个数为奇/偶数来判断目标的位置，目标必然在奇数的一侧。

注意需要判断中点是相同元素的前一个还是后一个，简化代码中处理中点必为前一个，减少了判断。

**补充：**

1. 二分法
2. 时间复杂度 `O(lg(n))`
3. 更简化的代码

    ```java
    public int singleNonDuplicate1(int[] nums) {
        
        int left = 0, right = nums.length - 1;
        
        while(left < right) {   // right 或 left 可能 = mid 时，判断条件不要有等号
            
            int mid = (left + right) / 2;
            
            if(mid % 2 == 1) {      // mid 永远取偶数位，只用判断后一位与其是否相同即可
                --mid;
            }
            
            if(mid < nums.length - 1 && nums[mid] == nums[mid + 1]) {   // 和 mid 后一位相同
                left = mid + 2;                                             // 左侧已为偶数个，找右侧，跳过相同的 mid, mid + 1
            } else {                                                    // 和 mid 后一位不同
                right = mid;                                                // 找左侧，可能为 mid
            }
        }
        
        return nums[right];
    }
    ```

## 代码

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        
        int left = 0, right = nums.length - 1;
        
        while(left <= right) {
            
            int mid = (left + right) / 2;
            
            if(mid > 0 && nums[mid] == nums[mid - 1]) {                         // 和 mid 前一位相同
                if((right - mid) % 2 == 0) {                                        // 右侧有偶数个元素
                    right = mid - 2;                                                    // 找左侧，跳过 mid - 1, mid
                } else {                                                            // 右侧有奇数个元素
                    left = mid + 1;                                                     // 找右侧，跳过 mid
                }
            } else if(mid < nums.length - 1 && nums[mid] == nums[mid + 1]) {    // 和 mid 后一位相同
                if((mid - left) % 2 == 0) {                                         // 左侧有偶数个元素
                    left = mid + 2;                                                     // 找右侧，跳过 mid, mid + 1
                } else {                                                            // 左侧有奇数个元素
                    right = mid - 1;                                                    // 找左侧，跳过 mid
                }
            } else {                                                            // 和 mid 前后都不同，返回
                return nums[mid];
            }
        }
        
        return -1;
    }
}
```
