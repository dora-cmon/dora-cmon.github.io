---
title: 【LeetCode】524. Longest Word in Dictionary through Deleting 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 双指针
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: f46d7949
date: 2021-05-14 16:10:47
---


# 问题描述

Given a string s and a string array dictionary, return the longest string in the dictionary that can be formed by deleting some of the given string characters. If there is more than one possible result, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

## 测试样例

```
Input: s = "abpcplea", dictionary = ["ale","apple","monkey","plea"]
Output: "apple"
```

```
Input: s = "abpcplea", dictionary = ["a","b","c"]
Output: "a"
```

## 说明

```
1 <= s.length <= 1000
1 <= dictionary.length <= 1000
1 <= dictionary[i].length <= 1000
s and dictionary[i] consist of lowercase English letters.
```

# 解题

## 思路

采用双指针来求解。

对于 `dictionary` 中的每一个字符串，将其与 `s` 进行匹配，若匹配，则判断其长度和字典序是否更优。

**补充：**

1. 双指针
1. 时间复杂度 `O(n)`

## 代码

```java
import java.util.Collections;
import java.util.List;

class Solution {
    public String findLongestWord(String s, List<String> dictionary) {
        // 结果
        String str_result = "";
        // 遍历 dictionary
        for(int i=0; i<dictionary.size(); ++i) {
            String str = dictionary.get(i);
            // s 指针， str 指针
            int idx_s = 0, idx_str = 0;
            // 与 s 进行匹配
            for(; idx_s<s.length(); ++idx_s) {
                // 已匹配完 str，跳出
                if(idx_str == str.length())     break;
                // 若 s 当前字母与 str 当前字母相同
                if(s.charAt(idx_s) == str.charAt(idx_str)) {
                    ++idx_str;      // str 进入下一字母
                }
            }
            
            if(idx_str == str.length()) {   // str 已被匹配完
                // 新串长度更长
                if(str.length() > str_result.length()) {        
                    str_result = str;
                }
                // 新串长度相等，字典序更小
                else if(str.length() == str_result.length() && str.compareTo(str_result) < 0) {   
                    str_result = str;
                }
            }
        }
        
        return str_result;
    }
}
```
