---
title: 【LeetCode】141. Linked List Cycle 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 链表
  - 双指针
  - 快慢指针
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: 7bfc535b
date: 2021-08-07 00:04:55
---

# 问题描述

Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

## 测试样例

![](/images/【LeetCode】141-Linked-List-Cycle-解题记录/2021-08-07-00-06-33.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

![](/images/【LeetCode】141-Linked-List-Cycle-解题记录/2021-08-07-00-06-58.png)

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

![](/images/【LeetCode】141-Linked-List-Cycle-解题记录/2021-08-07-00-07-21.png)

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

## 说明

```
The number of the nodes in the list is in the range [0, 10^4].
-10^5 <= Node.val <= 10^5
pos is -1 or a valid index in the linked-list.
```

# 解题

## 思路

使用快慢指针：
- 若链表可达 null 则无环
- 若快指针套圈慢指针则有环

**补充：**

1. 双指针
1. 快慢指针
1. 时间复杂度 `O(n)`

## 代码

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {   
        // 快慢指针
        ListNode slow = head, fast = head;
        
        // 若链表可达 null 则无环
        // 若快指针套圈慢指针则有环
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            
            if(slow == fast) {
                return true;
            }
        }
        
        return false;
    }
}
```
