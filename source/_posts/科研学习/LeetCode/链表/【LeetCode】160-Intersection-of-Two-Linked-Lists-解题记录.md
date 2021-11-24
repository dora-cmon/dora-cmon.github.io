---
title: 【LeetCode】160. Intersection of Two Linked Lists 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Easy
  - 链表
  - 算法
cover: /images/cover/leetcode.jpg
abbrlink: 50f23bee
date: 2021-08-06 23:32:34
---

# 问题描述

Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

For example, the following two linked lists begin to intersect at node c1:

![](/images/【LeetCode】160-Intersection-of-Two-Linked-Lists-解题记录/2021-08-06-23-43-19.png)

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

Note that the linked lists must retain their original structure after the function returns.

Custom Judge:

The inputs to the judge are given as follows (your program is not given these inputs):

- intersectVal - The value of the node where the intersection occurs. This is 0 if there is no intersected node.
- listA - The first linked list.
- listB - The second linked list.
- skipA - The number of nodes to skip ahead in listA (starting from the head) to get to the intersected node.
- skipB - The number of nodes to skip ahead in listB (starting from the head) to get to the intersected node.
The judge will then create the linked structure based on these inputs and pass the two heads, headA and headB to your program. If you correctly return the intersected node, then your solution will be accepted.

## 测试样例

![](/images/【LeetCode】160-Intersection-of-Two-Linked-Lists-解题记录/2021-08-06-23-59-58.png)

```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
```

![](/images/【LeetCode】160-Intersection-of-Two-Linked-Lists-解题记录/2021-08-07-00-00-21.png)

```
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Intersected at '2'
Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```

![](/images/【LeetCode】160-Intersection-of-Two-Linked-Lists-解题记录/2021-08-07-00-00-42.png)

```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: No intersection
Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
```

## 说明

```
The number of nodes of listA is in the m.
The number of nodes of listB is in the n.
0 <= m, n <= 3 * 10^4
1 <= Node.val <= 10^5
0 <= skipA <= m
0 <= skipB <= n
intersectVal is 0 if listA and listB do not intersect.
intersectVal == listA[skipA] == listB[skipB] if listA and listB intersect.
```

# 解题

## 思路

1. 获取链表长度
1. 调整起始指针，使得两链表后续长度相同
1. 向后遍历，首个地址相同的节点为交汇点

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
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null) {
            return null;
        }
        
        ListNode p1 = headA, p2 = headB;
        
        int len1 = getLength(p1), len2 = getLength(p2);
        int gap = Math.abs(len1 - len2);
        
        // p1 指向较长的链表
        if(len1 < len2) {
            p1 = headB;
            p2 = headA;
        }
        
        // p1 移动至和 p2 长度相同的位置
        while(gap-- > 0) {
            p1 = p1.next;
        }
        
        // 寻找地址相同的节点
        while(p1 != p2) {
            p1 = p1.next;
            p2 = p2.next;
        }
        
        return p1;
    }
    
    /**
     * 获得链表的长度
     */
    public int getLength(ListNode head) {
        ListNode p = head;
        
        int cnt = 0;
        while(p != null) {
            ++cnt;
            p = p.next;
        }
        
        return cnt;
    }
}
```