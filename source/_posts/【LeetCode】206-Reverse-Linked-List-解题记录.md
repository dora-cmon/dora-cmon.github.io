---
title: 【LeetCode】206. Reverse Linked List 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 链表
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: c7fc6e60
date: 2021-08-06 22:00:43
---


# 问题描述

Given the head of a singly linked list, reverse the list, and return the reversed list.

## 测试样例

![](images/【LeetCode】206-Reverse-Linked-List-解题记录/2021-08-06-21-54-07.png)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

![](images/【LeetCode】206-Reverse-Linked-List-解题记录/2021-08-06-21-54-59.png)

```
Input: head = [1,2]
Output: [2,1]
```

```
Input: head = []
Output: []
```

## 说明

```
The number of nodes in the list is the range [0, 5000].
-5000 <= Node.val <= 5000
```

# 解题

## 思路

记录 `pre`, `curr` 指针, 在遍历时记录每轮 `next` 指针，逆向即可。

**补充：**

1. 链表
1. 时间复杂度 `O(n)`

## 代码

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null, curr = head;
        while(curr != null) {
            ListNode next = curr.next;
            
            // 后移一位
            curr.next = pre;
            pre = curr;
            curr = next;
            
        }
        
        return pre;
    }
}
```