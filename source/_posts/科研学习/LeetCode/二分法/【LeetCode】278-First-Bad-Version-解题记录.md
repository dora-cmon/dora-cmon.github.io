---
title: 【LeetCode】278. First Bad Version 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 二分法
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: d2fc0325
date: 2021-08-08 22:44:00
---

# 问题描述

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

## 测试样例

```
Input: n = 5, bad = 4
Output: 4
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.
```

```
Input: n = 1, bad = 1
Output: 1
```

## 说明

```
1 <= bad <= n <= 2^31 - 1
```

# 解题

## 思路

使用二分法即可

**补充：**

1. 时间复杂度 `O(logn)`

1. 注意计算中值时，最好使用 `int mid = l + (r - l) / 2`，若使用 `mid = (l + r) / 2`，当数字非常大时，Integer 溢出，会导致错误！！！

## 代码

```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */

public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int l = 1, r = n;
        
        while(l < r) {
            // 如果使用 mid = (l + r) / 2
            // 数字非常大时，Integer 溢出，会导致错误！！！
            int mid = l + (r - l) / 2;
            
            if(isBadVersion(mid)) {
                r = mid;
            }
            else {
                l = mid + 1;
            }
        }
        
        return l;
    }
}
```


