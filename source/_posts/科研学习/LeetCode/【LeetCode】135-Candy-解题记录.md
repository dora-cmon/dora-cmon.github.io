---
title: 【LeetCode】135. Candy 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Hard
  - 贪心
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 86fcf368
date: 2021-04-23 17:54:22
---

# 问题描述

There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return the minimum number of candies you need to have to distribute the candies to the children.

## 测试样例

```
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
```

```
Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.
```

## 说明

```
n == ratings.length
1 <= n <= 2 * 104
0 <= ratings[i] <= 2 * 104
```

# 解题

## 思路

只需要简单的两次遍历即可：

1. 把所有孩子的糖果数初始化为 1；
   
2. 先从左往右遍历一遍，如果右边孩子的评分比左边的高，则右边孩子的糖果数更新为左边孩子的糖果数加 1；
   
3. 再从右往左遍历一遍，如果左边孩子的评分比右边的高，且左边孩子当前的糖果数不大于右边孩子的糖果数，则左边孩子的糖果数更新为右边孩子的糖果数加 1。

通过这两次遍历，分配的糖果就可以满足题目要求了。

这里的贪心策略即为，在每次遍历中，只考虑并更新相邻一侧的大小关系。

**补充：**

1. 贪心算法
2. 数组默认值填充方法 `Arrays.fill(array, value)`
3. 数组求和方法 `IntStream.of(array).sum()`
4. 时间复杂度 `O(n)`

## 代码

```java
import java.util.Arrays;
import java.util.stream.IntStream;

class Solution {
    public int candy(int[] ratings) {

        // 初始化 candies 数组，表示每个人的糖果
        int[] candies = new int[ratings.length];
        Arrays.fill(candies, 1);    // 最少为 1 个糖果，初始化为 1

        // 从左至右遍历，保证右侧分数高的孩子糖果多 1
        for(int i=0; i<ratings.length-1; ++i) {
            if(ratings[i+1] > ratings[i])
                candies[i+1] = candies[i] + 1;
        }
        // 从右至左遍历，保证左侧分数高，且糖果数小于等于右侧的孩子糖果多 1
        for(int i=ratings.length-1; i>0; --i) {
            if(ratings[i-1] > ratings[i] && candies[i-1] <= candies[i])
                candies[i-1] = candies[i] + 1;
        }
        // candies 数组求和
        return IntStream.of(candies).sum();
    }
}
```
