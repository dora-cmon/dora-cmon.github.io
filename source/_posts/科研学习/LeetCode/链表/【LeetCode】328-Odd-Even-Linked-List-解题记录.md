---
title: 【LeetCode】328. Odd Even Linked List 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 链表
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 86356f80
date: 2021-08-08 21:49:46
---

# 问题描述

Given the head of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return the reordered list.

The first node is considered odd, and the second node is even, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in O(1) extra space complexity and O(n) time complexity.

## 测试样例

![](/images/【LeetCode】328-Odd-Even-Linked-List-解题记录/2021-08-08-21-51-45.png)

```
Input: head = [1,2,3,4,5]
Output: [1,3,5,2,4]
```

![](/images/【LeetCode】328-Odd-Even-Linked-List-解题记录/2021-08-08-21-52-08.png)

```
Input: head = [2,1,3,5,6,4,7]
Output: [2,3,6,7,1,5,4]
```

## 说明

```
n == number of nodes in the linked list
0 <= n <= 10^4
-10^6 <= Node.val <= 10^6
```

# 解题

## 思路

分别构造奇数位链表和偶数位链表，步长均为 `2`，构造完成后将偶数位链表附加到奇数位链表之后。

**补充：**

1. 链表
1. 时间复杂度 `O(n)`

## 代码

法一：
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
    public ListNode oddEvenList(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        
        // 奇数链表，偶数链表
        ListNode odd = head, even = head.next;
        // 偶数链表前驱节点
        ListNode evenPre = new ListNode(-1, even);
        
        // 构造两个新链表，步长均为 2
        while(odd.next != null && even.next != null) {
            odd.next = odd.next.next;
            even.next = even.next.next;
            
            odd = odd.next;
            even = even.next;
        }
        
        // 将偶数链表附加到奇数链表后
        odd.next = evenPre.next;
        
        return head;
    }
}
```