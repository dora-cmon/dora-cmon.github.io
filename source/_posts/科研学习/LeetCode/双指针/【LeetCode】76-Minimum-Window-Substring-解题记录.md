---
title: 【LeetCode】76. Minimum Window Substring 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Hard
  - 双指针
  - 滑动窗口
  - 字符串
  - 算法
cover: /images/cover/leetcode.jpg
abbrlink: 4a667ae0
date: 2021-05-12 16:03:33
---


# 问题描述

Given two strings s and t of lengths m and n respectively, return the minimum window in s which will contain all the characters in t. If there is no such window in s that covers all characters in t, return the empty string "".

Note that If there is such a window, it is guaranteed that there will always be only one unique minimum window in s.

## 测试样例

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
```

```
Input: s = "a", t = "a"
Output: "a"
```

## 说明

```
m == s.length
n == t.length
1 <= m, n <= 10^5
s and t consist of English letters.
```

# 解题

## 思路

使用滑动窗口求解，两个指针 `left` 和 `right` 都从最左端向最右端移动，且 `left` 的位置一定在 `right` 的左边或重合。

1. `flag_chars` 记录每个字符是否在 `t` 中存在；`cnt_chars` 记录目前每个字符缺少的数量，
1. 移动窗口右边界，边界字符在 `t` 中出现过，该字符缺少的数量 `cnt_chars[c]` `-1`，若缺少数量 `>=0` 则该字符有效（若 `<0` 表示窗口中该字符数量大于 `t` 中该字符数量），有效数字 `cnt_valid_char` 计数 `+1`，
1. 若窗口中有效字符数与 t 中相同，得到符合题目的一个解，将其与最小解比较并记录。此时，移动窗口左边界，直到窗口不符合题目，退出内层循环。

**补充：**

1. 双指针
1. 滑动窗口
1. 使用长度为 `128` 的数组来映射字符
1. 需要单独判断无解的情况 `flag_ok`
1. 时间复杂度 `O(n)`
    虽然在 `for` 循环里出现了一个 `while` 循环，但是因为 `while` 循环负责移动 `left` 指针，且 `left` 只会从左到右移动一次，因此总时间复杂度仍然是 `O(n)`

## 代码

```java
import java.util.Arrays;

class Solution {
    public String minWindow(String s, String t) {
        // 子串 t 中各字符是否出现过
        boolean[] flag_chars = new boolean[128];
        // 保存子串 t 中各字符的数量
        int[] cnt_chars = new int[128];
        
        // 统计 t 中出现的字符
        for(int i=0; i<t.length(); ++i) {
            char c = t.charAt(i);
            // 出现标志
            flag_chars[c] = true;
            // 对应字符计数器 +1
            ++cnt_chars[c];
        }
        
        // 合法窗口是否出现过
        boolean flag_ok = false;
        // 滑动窗口
        int left = 0, right = 0;
        // 窗口中出现在子串 t 中字符数量
        int cnt_valid_char = 0;
        // 记录最小窗口左边界及大小
        int min_left = 0, min_size = s.length();
        // 移动窗口右边界
        for(; right<s.length(); ++right) {
            char c = s.charAt(right);
            // 当前字符在 t 中出现过，且该字符有效，计数
            if(flag_chars[c] == true && --cnt_chars[c] >= 0) {
                ++cnt_valid_char;
            }
            // 窗口中有效字符数与 t 中字符数相同（包含所有 t 中字符且满足数量）
            while(cnt_valid_char == t.length()) {
                flag_ok = true;
                // 判断当前窗口是否为最小
                if(right - left + 1 < min_size) {
                    min_left = left;
                    min_size = right - left + 1;
                }
                // 从窗口中删除左边界元素
                c = s.charAt(left);
                // 当前字符在 t 中出现过，从窗口中移出一个有效字符
                if(flag_chars[c] == true && ++cnt_chars[c] > 0) {
                    --cnt_valid_char;
                }
                // 窗口左边界向右移一位
                ++left;
            }
        }
        
        return flag_ok == true ? s.substring(min_left, min_left + min_size) : "";
    }
}
```
