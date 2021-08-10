---
title: 【LeetCode】346. Moving Average from Data Stream 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 队列
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: f4a86a0d
date: 2021-08-10 12:51:30
---

# 问题描述

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

## 测试样例

```
MovingAverage m = new MovingAverage(3);
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3
```

# 解题

## 思路

使用队列来保存窗口数据，保持队列小于等于窗口大小，并记录窗口元素值方便计算平均值。

**补充：**

1. 队列
2. 时间复杂度 `O(n)`

## 代码

```java
import java.util.Queue;

public class MovingAverage {

    private Queue<Integer> queue;
    private int window_sz;
    private double window_sum;

    /*
    * @param size: An integer
    */public MovingAverage(int size) {
        // do intialization if necessary
        queue = new LinkedList<Integer>();
        window_sz = size;
        window_sum = 0;
    }

    /*
     * @param val: An integer
     * @return:  
     */
    public double next(int val) {
        // 容量满时，弹出队首元素
        if(queue.size() == window_sz) {
            window_sum -= queue.poll();
        }
        
        // 新元素加入队尾
        queue.offer(val);
        window_sum += val;

        return window_sum / queue.size();
    }
}
```

