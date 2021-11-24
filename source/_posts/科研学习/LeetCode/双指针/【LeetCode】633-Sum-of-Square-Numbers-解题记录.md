---
title: 【LeetCode】633. Sum of Square Numbers 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 双指针
  - 算法
cover: /images/cover/leetcode.jpg
abbrlink: 66d12b72
date: 2021-05-14 12:41:13
---


# 问题描述

Given a non-negative integer c, decide whether there're two integers a and b such that a2 + b2 = c.

## 测试样例

```
Input: c = 5
Output: true
Explanation: 1 * 1 + 2 * 2 = 5
```

```
Input: c = 3
Output: false
```

```
Input: c = 4
Output: true
```

```
Input: c = 2
Output: true
```

```
Input: c = 1
Output: true
```

## 说明

```
0 <= c <= 2^31 - 1
```

# 解题

## 思路

采用双指针来求解。左侧从 0 开始，右侧最大为 `sqrt(c)` ：

1. 两数平方和等于给定值，那么它们就是我们要的结果。
1. 两数平方和小于给定值，把左边的指针右移一位，使得当前的平方和增加一点。
1. 两数平方和大于给定值，把右边的指针左移一位，使得当前的平方和减少一点。

**补充：**

1. 双指针
1. `double` 转 `int` 方法：`(new Double(varible)).intValue()`
1. 时间复杂度 `O(n)`

## 代码

```java
class Solution {
    public boolean judgeSquareSum(int c) {
        // a < b，且 b 不超过该值
        int a = 0, b = (new Double(Math.sqrt(c))).intValue();
        
        while(a <= b) {
            int value = a * a + b * b;
            
            if(value == c) {        // 符合题意，返回结果
                return true;
            } else if(value < c) {  // a + 1，平方和增大
                ++a;
            } else {                // b - 1，平方和减小
                --b;
            }
        }
        
        return false;
    }
}
```
