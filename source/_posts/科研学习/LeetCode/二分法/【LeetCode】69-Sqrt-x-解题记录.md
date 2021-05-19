---
title: 【LeetCode】69. Sqrt(x) 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpeg
katex: true
abbrlink: 340c2b50
date: 2021-05-18 18:09:54
---

# 问题描述

Given a non-negative integer x, compute and return the square root of x.

Since the return type is an integer, the decimal digits are truncated, and only the integer part of the result is returned.

## 测试样例

```
Input: x = 4
Output: 2
```

```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
```

## 说明

```
0 <= x <= 2^31 - 1
```

# 解题

## 思路

对 `[0, x]` 区间使用二分法求解。

为了防止除以 `0`，把 `x = 0` 的情况单独考虑，然后对区间 `[1, a]` 进行二分查找。

**补充：**

1. 二分法
1. 时间复杂度 `O(lg(n))`
1. 还有一种更快的算法 —— 牛顿迭代法，其公式为 $x_{n+1} = x_n - f(x_n) / f^'(x_n)$。给定 $f(x) = x^2 - a = 0$，这里的迭代公式为 $x_{n+1} = (x_n + a/x_n)/2$，其代码如下。

    ```java
    class Solution {
        public int mySqrt(int x) {
            long a = x;
            while (a * a > x) {
                a = (a + x / a) / 2;
            }
            
            return (new Long(a)).intValue();
        }
    }
    ```
    为了防止 `int` 超上界，使用 `long` 来存储乘法结果

## 代码

```java
class Solution {
    public int mySqrt(int x) {
        // 特殊情况
        if(x == 0)  return 0;
        // 二分，left 不能从 0 开始，否则会除 0
        int left = 1, right = x;
        while(left <= right) {
            // 取中值
            int mid = (left + right) / 2;
            // 向下取整的开方值
            int sqrt = x / mid;
            
            if(sqrt == mid)  {       // 找到结果
                return mid;
            } else if(mid < sqrt) { // 小了，取右半边
                left = mid + 1;
            } else {                    // 大了，取左半边
                right = mid - 1;
            }
        }
        // left > right，即到了 left - 1 == right 的情况，向下取整，故取 right
        return right;
    }
}
```
