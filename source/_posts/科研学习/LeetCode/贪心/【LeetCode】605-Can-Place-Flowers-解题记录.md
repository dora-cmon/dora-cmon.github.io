---
title: 【LeetCode】605. Can Place Flowers 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 贪心
cover: /images/cover/leetcode.jpeg
abbrlink: ef5a23d
date: 2021-04-23 17:54:45
---

# 问题描述

You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in adjacent plots.

Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule.

## 测试样例

```
Input: flowerbed = [1,0,0,0,1], n = 1
Output: true
```

```
Input: flowerbed = [1,0,0,0,1], n = 2
Output: false
```

## 说明

```
1 <= flowerbed.length <= 2 * 104
flowerbed[i] is 0 or 1.
There are no two adjacent flowers in flowerbed.
0 <= n <= flowerbed.length
```

# 解题

## 思路

遍历 flower bed 只要遇到可以放入的位置，放入即可

**补充：**

1. 注意处理第一个和最后一个位置
1. 时间复杂度 `O(n)`

## 代码

```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        // flower bed 为空或长度不可能放下 n 个植物
        if(flowerbed == null || flowerbed.length == 0 || flowerbed.length < n / 2) {
            return false;
        }
        
        // 可放入的植物数量
        int cnt = 0;
        
        // 遍历 flower bed
        for(int i=0; i<flowerbed.length; ++i) {
            // 若当前位置为空
            if(flowerbed[i] == 0) {
                // 前一个位置
                int pre = (i == 0 ? 0 : flowerbed[i-1]);
                // 后一个位置
                int next = (i == flowerbed.length-1 ? 0 : flowerbed[i+1]);
                // 若前后均为空，放入
                if(pre == 0 && next == 0) {
                    flowerbed[i] = 1;
                    ++cnt;
                }
                // 已满足条件，提前退出
                if(cnt >= n) {
                    break;
                }
            }
        }
        
        return cnt >= n ? true : false;
    }
}
```