---
title: 【LeetCode】179. Largest Number 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 排序
  - 算法
cover: /images/cover/leetcode.jpg
abbrlink: bf39723
date: 2021-08-08 22:03:07
---

# 问题描述

Given a list of non-negative integers nums, arrange them such that they form the largest number.

Note: The result may be very large, so you need to return a string instead of an integer.

## 测试样例

```
Input: nums = [10,2]
Output: "210"
```

```
Input: nums = [3,30,34,5,9]
Output: "9534330"
```

```
Input: nums = [1]
Output: "1"
```

```
Input: nums = [1]
Output: "1"
```

## 说明

```
1 <= nums.length <= 100
0 <= nums[i] <= 10^9
```

# 解题

## 思路

将数组转换为字符串，并对其进行排序，规则为判断两字符串分别在前时的字母序大小，即 `（str1 + str2).compareTo(str2 + str1)`，最后将数组中的字符串按顺序连接即可。

**补充：**

1. 时间复杂度 `O(logn)`
1. 使用 lambda 排序
1. 注意处理前缀的 `0` 以及结果为 `0` 的情况。

## 代码

```java
import java.util.Collections;

class Solution {
    public String largestNumber(int[] nums) {
        // int 数组转 string 列表
        List<String> str_nums = new ArrayList<String>();
        for(int num : nums) {
            str_nums.add(Integer.toString(num));
        }
        
        // 排序
        Collections.sort(str_nums, (a, b) -> (b + a).compareTo(a + b));
        
        // 构造结果
        String rt = "";
        for(String s : str_nums) {
            if(!(s.equals("0") && rt.equals(""))) {
                rt += s;
            }
        }
        
        return rt.equals("") ? "0" : rt;
    }
}
```


