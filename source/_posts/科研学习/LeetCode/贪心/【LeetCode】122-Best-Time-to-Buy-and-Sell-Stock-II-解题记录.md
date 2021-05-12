---
title: 【LeetCode】122. Best Time to Buy and Sell Stock II 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 贪心
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: e513e7dc
date: 2021-05-07 18:32:42
---

# 问题描述

You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

## 测试样例

```
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```

```
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
```

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e., max profit = 0.
```

## 说明

```
1 <= prices.length <= 3 * 104
0 <= prices[i] <= 104
```

# 解题

## 思路

若当天股票价格比前一天高，则在前一天买入，当天卖出即可。

**补充：**

1. 贪心算法
1. 时间复杂度 `O(n)`

## 代码

```java
class Solution {
    public int maxProfit(int[] prices) {
        // 特殊情况，0 或 1 天
        if(prices.length <= 1)  return 0;
        // 收益
        int profit = 0;
        for(int i=1; i<prices.length; ++i) {
            // 后一天相对前一天上涨，上涨额加入收益
            if(prices[i] > prices[i - 1])
                profit += (prices[i] - prices[i - 1]);
        }
        return profit;
    }
}
```

