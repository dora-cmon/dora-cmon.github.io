---
title: 【LeetCode】167. Two Sum II - Input array is sorted 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 双指针
  - 算法
cover: /images/cover/leetcode.jpg
abbrlink: 92ab2227
date: 2021-05-10 17:35:28
---


# 问题描述

Given an array of integers numbers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

Return the indices of the two numbers (1-indexed) as an integer array answer of size 2, where 1 <= answer[0] < answer[1] <= numbers.length.

You may assume that each input would have exactly one solution and you may not use the same element twice.

## 测试样例

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

```
Input: numbers = [2,3,4], target = 6
Output: [1,3]
```

```
Input: numbers = [-1,0], target = -1
Output: [1,2]
```

## 说明

```
2 <= numbers.length <= 3 * 10^4
-1000 <= numbers[i] <= 1000
numbers is sorted in increasing order.
-1000 <= target <= 1000
Only one valid answer exists.
```

# 解题

## 思路

数组已经排好序，可以采用双指针来求解。两个指针分别指向首、尾元素，并沿反方向遍历，可能出现三种情况：

1. 两个指针指向元素的和等于给定值，那么它们就是我们要的结果。
1. 两个指针指向元素的和小于给定值，把左边的指针右移一位，使得当前的和增加一点。
1. 两个指针指向元素的和大于给定值，把右边的指针左移一位，使得当前的和减少一点。

**补充：**

1. 双指针
1. 时间复杂度 `O(n)`
1. 对于排好序且有解的数组，双指针一定能遍历到最优解

    假设最优解的两个数的位置分别是 `l` 和 `r`。
    
    - 假设在左指针在 `l` 左边的时候，右指针已经移动到了 `r`；此时两个指针指向值的和小于给定值，因此左指针会一直右移直到到达 `l`。
    - 假设在右指针在 `r` 右边的时候，左指针已经移动到了 `l`；此时两个指针指向值的和大于给定值，因此右指针会一直左移直到到达 `r`。
    
    所以双指针在任何时候都不可能处于 `(l,r)` 之间，又因为不满足条件时指针必须移动一个，所以最终一定会收敛在 `l` 和 `r`。

## 代码

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        // 结果
        int[] result = new int[2];
        // 首尾指针
        int idx_head = 0, idx_tail = numbers.length - 1;
        
        while(idx_head < idx_tail) {
            int sum_2 = numbers[idx_head] + numbers[idx_tail];
            
            if(sum_2 == target)         // = target，返回结果
                return new int[]{idx_head + 1, idx_tail + 1};
            else if(sum_2 < target)     // < target，首指针后移一位
                ++idx_head;
            else                        // > target，尾指针前移一位
                --idx_tail;
        }
        // 题目保证有解，不可能到这里
        return new int[2];
    }
}
```
