---
title: 【LeetCode】680. Valid Palindrome II 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 双指针
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 6a28d588
date: 2021-05-14 13:20:39
---



# 问题描述

Given a string s, return true if the s can be palindrome after deleting at most one character from it.

## 测试样例

```
Input: s = "aba"
Output: true
```

```
Input: s = "abca"
Output: true
Explanation: You could delete the character 'c'.
```

```
Input: s = "abc"
Output: false
```

## 说明

```
1 <= s.length <= 10^5
s consists of lowercase English letters.
```

# 解题

## 思路

采用双指针来求解。两个指针分别从两端向中间遍历，遇到不相同的字符时，分别剔除左侧和右侧的一个元素，判断其余子串是否回文。

**补充：**

1. 双指针
1. `substring()` 函数是左闭右开区间
1. 时间复杂度 `O(n)`

## 代码

```java
class Solution {
    
    private boolean flag_delete = false;    // 是否已经删除一个元素
    
    public boolean validPalindrome(String s) {
        // 左右指针
        int left = 0, right = s.length() - 1;
        // 两端向中间遍历
        while(left < right) {
            if(s.charAt(left) != s.charAt(right)) {   // 字符不相等
                if(flag_delete == true) {   // 已经删除一次，第二次出现，返回
                    return false;
                }
                // 设置出现标志
                flag_delete = true;
                // 检测分别去掉左右元素时，剩余子串是否回文
                // substring 为左闭右开区间
                return validPalindrome(s.substring(left + 1, right + 1)) || validPalindrome(s.substring(left, right));
            }
            ++left;
            --right;
        }
        // 全部找完，符合题意
        return true;
    }
}
```
