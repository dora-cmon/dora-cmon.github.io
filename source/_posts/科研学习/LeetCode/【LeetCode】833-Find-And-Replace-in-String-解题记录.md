---
title: 【LeetCode】833. Find And Replace in String 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Middle
  - String
  - 字符串
date: 2021-04-23 17:54:45
---


# 问题描述

To some string S, we will perform some replacement operations that replace groups of letters with new ones (not necessarily the same size).

Each replacement operation has 3 parameters: a starting index i, a source word x and a target word y.  The rule is that if x starts at position i in the original string S, then we will replace that occurrence of x with y.  If not, we do nothing.

For example, if we have S = "abcd" and we have some replacement operation i = 2, x = "cd", y = "ffff", then because "cd" starts at position 2 in the original string S, we will replace it with "ffff".

Using another example on S = "abcd", if we have both the replacement operation i = 0, x = "ab", y = "eee", as well as another replacement operation i = 2, x = "ec", y = "ffff", this second operation does nothing because in the original string S[2] = 'c', which doesn't match x[0] = 'e'.

All these operations occur simultaneously.  It's guaranteed that there won't be any overlap in replacement: for example, S = "abc", indexes = [0, 1], sources = ["ab","bc"] is not a valid test case.

## 测试样例

```
Input: S = "abcd", indexes = [0, 2], sources = ["a", "cd"], targets = ["eee", "ffff"]
Output: "eeebffff"
Explanation:
"a" starts at index 0 in S, so it's replaced by "eee".
"cd" starts at index 2 in S, so it's replaced by "ffff".
```

```
Input: S = "abcd", indexes = [0, 2], sources = ["a", "cd"], targets = ["eee", "ffff"]
Output: "eeebffff"
Explanation:
"a" starts at index 0 in S, so it's replaced by "eee".
"cd" starts at index 2 in S, so it's replaced by "ffff".
```

## 说明

```
0 <= S.length <= 1000
S consists of only lowercase English letters.
0 <= indexes.length <= 100
0 <= indexes[i] < S.length
sources.length == indexes.length
targets.length == indexes.length
1 <= sources[i].length, targets[i].length <= 50
sources[i] and targets[i] consist of only lowercase English letters.
```

# 解题

## 思路

遍历字符串，若当前位在 `indexes` 中，判断是否需要替换，

- 若需要替换，则将 `target` 字符串 `append` 入结果，`idx = idx + target.length - 1`
- 若不需要替换，则将当前字符 `append` 入结果

返回结果字符串。

**补充：**

1. 用 Map 来保存替换规则，降低查找时间
2. `i = i + target.length() - 1`
3. 时间复杂度 `O(n)`

## 代码

```java
import java.util.Map;
import java.util.HashMap;

class Record {
    public String source;
    public String target;

    public Record(String s, String t) {
        this.source = s;
        this.target = t;
    }
}

class Solution {
    
    // 结果
    private StringBuilder sb = new StringBuilder();
    // 替换字典
    private Map<Integer, Record> map = new HashMap<Integer, Record>();
    
    public String findReplaceString(String S, int[] indexes, String[] sources, String[] targets) {

        // 存储替换字典
        for(int i=0; i<indexes.length; ++i) {
            map.put(indexes[i], new Record(sources[i], targets[i]));
        }

        // 遍历字符串
        for(int i=0; i<S.length(); ++i) {
            // 当前位需判断是否替换
            if(map.containsKey(i)) {
                String source = map.get(i).source, target = map.get(i).target;
                int source_len = source.length(), target_len = target.length();
                // 匹配，进行替换
                if(source.equals(S.substring(i, i+source_len))) {
                    sb.append(target);
                    i = i + source_len - 1;
                }
                // 不匹配，不进行替换
                else {
                    sb.append(S.charAt(i));
                }
            }
            // 当前位不需要替换
            else {
                sb.append(S.charAt(i));
            }
        }
        return sb.toString();
    }
    
}
```