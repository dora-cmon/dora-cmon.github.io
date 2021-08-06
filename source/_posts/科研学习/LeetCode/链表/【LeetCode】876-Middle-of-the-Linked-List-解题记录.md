---
title: 【LeetCode】876. Middle of the Linked List 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 链表
  - 快慢指针
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: a52b91a6
date: 2021-08-06 23:25:44
---


# 问题描述

Given the head of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

## 测试样例

![](/images/【LeetCode】876-Middle-of-the-Linked-List-解题记录/2021-08-06-23-28-55.png)

```
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.
```

![](/images/【LeetCode】876-Middle-of-the-Linked-List-解题记录/2021-08-06-23-29-37.png)

```
Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
```

## 说明

```
The number of nodes in the list is in the range [1, 100].
1 <= Node.val <= 100
```

# 解题

## 思路

使用快慢指针，快指针到达链表尾时，慢指针正好为链表中央。

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
    public ListNode middleNode(ListNode head) {
        // 快慢指针
        ListNode slow = head, fast = head;
        
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;
    }
}
```