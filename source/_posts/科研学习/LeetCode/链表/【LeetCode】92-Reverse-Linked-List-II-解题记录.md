---
title: 【LeetCode】92. Reverse Linked List II 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 链表
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: aee51397
date: 2021-08-07 00:10:42
---

# 问题描述

Given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right, and return the reversed list.

## 测试样例

![](/images/【LeetCode】92-Reverse-Linked-List-II-解题记录/2021-08-07-00-12-11.png)

```
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

```
Input: head = [5], left = 1, right = 1
Output: [5]
```

## 说明

```
The number of nodes in the list is n.
1 <= n <= 500
-500 <= Node.val <= 500
1 <= left <= right <= n
```

# 解题

## 思路

以 `1 -> 2 -> 3 -> 4 -> 5` , `left = 2, right = 5` 为例：

1. 找到需要逆序的区间头尾节点，以及头前、尾后节点

    ```
    1->|2->3->4|->5
    ^   ^     ^   ^
    |   |     |   |
    前  头    尾  后
    ```

1. 取出区间头

    ```
    1->|3->4|->5
    ^   ^  ^   ^
    |   |  |   |
    前  2  尾  后
        ^
        |
        头
    ```

1. 区间头附加到区间尾后

    ```
    1->|3->4|->2->5
    ^      ^   ^  ^
    |      |   |  |
    前     尾  头  后
    ```

1. 更新区间尾后
    ```
    1->|3->4|->2->5
    ^      ^   ^
    |      |   |
    前     尾  头
               ^
               |
               后
    ```

1. 更新区间头

    ```
    1->|3->4|->2->5
    ^   ^  ^   ^
    |   |  |   |
    前  头 尾  后

    ```

1. 循环以上步骤，直到 `前 -> 尾`

    ```
    1->|4|->3->2->5
    ^   ^
    |   |
    前  尾
    ```



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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        
        // 统一处理 left = 1
        ListNode headPre = new ListNode(-1, head);
        
        int idx = 1;
        ListNode p = head, pre = headPre;
        
        // 寻找区间起始
        while(idx < left) {
            pre = p;
            p = p.next;
            ++idx;
        }
        // 区间头，头前
        ListNode betweenHead = p, betweenHeadPre = pre;
        
        // 寻找区间结尾
        while(idx <= right) {
            pre = p;
            p = p.next;
            ++idx;
        }
        // 区间尾，尾后
        ListNode betweenTail = pre, betweenTailBack = p;
        
        while(betweenHeadPre.next != betweenTail) {
            // 区间头取出
            betweenHeadPre.next = betweenHead.next;
            
            // 区间头附加到区间尾后
            betweenTail.next = betweenHead;
            // 更新区间尾后
            betweenHead.next = betweenTailBack;
            betweenTailBack = betweenHead;
            
            // 更新区间头
            betweenHead = betweenHeadPre.next;
        }
        
        return headPre.next;
    }
}
```

法二：

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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        
        ListNode headPre = new ListNode(-1, head);
        
        int idx = 1;
        ListNode p = head, pre = headPre;
        
        // 寻找区间起始
        while(idx < left) {
            pre = p;
            p = p.next;
            ++idx;
        }
        ListNode betweenHead = p, betweenHeadPre = pre;
        
        // 寻找区间结尾
        while(idx <= right) {
            pre = p;
            p = p.next;
            ++idx;
        }
        ListNode betweenTail = pre, betweenTailBack = p;
        
        betweenTail.next = null;
        ListNode reverseHead = reverseLinkedList(betweenHead);
        
        // 拼接
        betweenHeadPre.next = reverseHead;

        // 头变尾，附加后续节点
        betweenHead.next = betweenTailBack;
        
        return headPre.next;
    }
    
    /**
     * 翻转链表
     */
    public ListNode reverseLinkedList(ListNode head) {
        ListNode pre = null, curr = head;
        while(curr != null) {
            ListNode back = curr.next;
            
            curr.next = pre;
            pre = curr;
            curr = back;
        }
        return pre;
    }
}
```