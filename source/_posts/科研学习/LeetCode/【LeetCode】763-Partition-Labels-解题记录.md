---
title: 【LeetCode】763. Partition Labels 解题记录 - 科研学习 - LeetCode
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 贪心
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: c336fcb2
date: 2021-05-08 00:04:14
---

# 问题描述

A string S of lowercase English letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.

## 测试样例

```
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
```

## 说明

```
S will have length in range [1, 500].
S will consist of lowercase English letters ('a' to 'z') only.
```

# 解题

## 思路

字符串 S 中相同的字母只能在同一个子串中，

1. 首先记录每个字母最后出现的位置，
2. 用 `sub_str_begin` 和 `sub_str_end` 分别表示当前子串的起始位置和结束位置，每遍历一个字母，将 `sub_str_end` 更新为当前字母最后位置，和原结束位置中的较大值（一旦当前子串包含一个字母，则必须包含所有的该字母），
3. 当 `i == sub_str_end` 时，表示该字串已结束，计算 `sub_str_end - sub_str_begin` 子串长度加入结果列表，并更新 `sub_str_begin` 和 `sub_str_end` 至后续位置，继续步骤 2

**补充：**

1. 贪心算法
2. 时间复杂度 `O(n)`

## 代码

```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    public List<Integer> partitionLabels(String S) {
        // 记录结果，截取 letter 的长度
        List<Integer> lst_result = new ArrayList<Integer>();
        
        // 记录每个字母最后出现的位置
        int[] letter_last_appear = new int[26];
        for(int i=0; i<S.length(); ++i) {
            letter_last_appear[S.charAt(i) - 'a'] = i;
        }
        
        // 子串起始位置和结束位置
        int sub_str_begin = 0, sub_str_end = 0;
        for(int i=0; i<S.length(); ++i) {
            // 更新子串结束位置为字串中所有字母最后出现位置的最大值
            int letter_end = letter_last_appear[S.charAt(i) - 'a'];
            if(letter_end > sub_str_end)
                sub_str_end = letter_end;
            
            // 若当前位置与子串结束位置相等，分割
            if(i == sub_str_end) {
                // 记录结果
                lst_result.add(sub_str_end - sub_str_begin + 1);
                // 下一个子串
                sub_str_begin = i + 1;
                sub_str_end = i + 1;
            }
        }
        return lst_result;
    }
}
```
