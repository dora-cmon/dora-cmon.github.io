---
title: 【LeetCode】406. Queue Reconstruction by Height 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 贪心
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 1d2ecea5
date: 2021-05-08 16:38:47
---


# 问题描述

You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order). Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi.

Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).

## 测试样例

```
Input: people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
Output: [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
Explanation:
Person 0 has height 5 with no other people taller or the same height in front.
Person 1 has height 7 with no other people taller or the same height in front.
Person 2 has height 5 with two persons taller or the same height in front, which is person 0 and 1.
Person 3 has height 6 with one person taller or the same height in front, which is person 1.
Person 4 has height 4 with four people taller or the same height in front, which are people 0, 1, 2, and 3.
Person 5 has height 7 with one person taller or the same height in front, which is person 1.
Hence [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] is the reconstructed queue.
```

```
Input: people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
Output: [[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
```

## 说明

```
1 <= people.length <= 2000
0 <= hi <= 106
0 <= ki < people.length
It is guaranteed that the queue can be reconstructed.
```

# 解题

## 思路

1. 首先按照身高从高到低，对队列排序，若身高相同，则 k_i 小的排前面，
2. 新建一个空的数组 `queue`，保存结果，
3. 遍历排序的数组，将每一项插入到 `queue` 数组中的第 `k_i` 位置

    - 相同身高的人会按 `k_i` 从小到大加入
    - 当加入身高为 `h_i` 的人时，由于原队列已经排序，结果队列中已加入的人的身高必然 >= `h_i`，因此，第 `k_i` 位置即为应该加入的位置（前方有 `k_i` 个人的身高 >= `h_i`）

**补充：**

1. 贪心算法
2. 时间复杂度 `O(nlogn)`（排序）

## 代码

```java
import java.util.Arrays;

class Solution {
    // 新队列
    int[][] queue;
    // 新队列人数
    int cnt_people = 0;
    
    public int[][] reconstructQueue(int[][] people) {
        // 构造新队列
        queue = new int[people.length][2];
        
        // 按高度 h_i 排序原队列
        Arrays.sort(people, (int[] p1, int[] p2) -> p2[0] == p1[0] ? p1[1] - p2[1] : p2[0] - p1[0]);
        
        // 遍历原队，插入新队
        for(int i=0; i<people.length; ++i) {
            // 插入第 k_i 个位置
            insert(queue, people[i][1], people[i]);
        }
        return queue;
    }
    
    public int insert(int[][] queue, int idx, int[] lst_pair) {
        // 替换第 idx 位置为 lst_pair
        int[] pre = queue[idx];
        queue[idx] = lst_pair;
        // 全部后移一位
        for(int i=idx+1; i<=cnt_people; ++i) {
            int[] tmp = queue[i];
            queue[i] = pre;
            pre = tmp;
        }
        ++cnt_people;
        // 返回被替换的坐标
        return idx;
    }
}
```
