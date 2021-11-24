---
title: 【LeetCode】147. Insertion Sort List 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 排序
  - 算法
cover: /images/cover/leetcode.jpg
abbrlink: 6a9dec7e
date: 2021-06-01 17:11:22
---


# 问题描述

Given the head of a singly linked list, sort the list using insertion sort, and return the sorted list's head.

The steps of the insertion sort algorithm:

1. Insertion sort iterates, consuming one input element each repetition and growing a sorted output list.
2. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list and inserts it there.
3. It repeats until no input elements remain.

The following is a graphical example of the insertion sort algorithm. The partially sorted list (black) initially contains only the first element in the list. One element (red) is removed from the input data and inserted in-place into the sorted list with each iteration.

## 测试样例

![](/images/【LeetCode】147-Insertion-Sort-List-解题记录/2021-06-01-17-05-51.png)

```
Input: head = [4,2,1,3]
Output: [1,2,3,4]
```
![](/images/【LeetCode】147-Insertion-Sort-List-解题记录/2021-06-01-17-07-24.png)

```
Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
```

## 说明

```
nums1.length == m
nums2.length == n
0 <= m <= 1000
0 <= n <= 1000
1 <= m + n <= 2000
-10^6 <= nums1[i], nums2[i] <= 10^6
```

# 解题

## 思路

插入排序链表版，添加头节点前驱可以简化代码。

**补充：**

1. 时间复杂度 `O(n^2)`

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
    public ListNode insertionSortList(ListNode head) {
        // 为原始链表头节点添加前驱
        ListNode head_origin_pre = new ListNode(-1, head);
        // 结果链表的前驱
        ListNode head_new_pre = new ListNode(-1);
        
        ListNode p = head_origin_pre.next;      // 从第一个元素开始
        while(p != null) {
            // 从原始链表中，剔除正在待插入元素
            head_origin_pre.next = p.next;
            // 结果链表中，正在与待插入元素作比较的元素及其前驱
            ListNode p_insert_pre = head_new_pre;
            ListNode p_insert = head_new_pre.next;
            // 结果链表中元素值小于待插入元素，向后移动
            while(p_insert != null && p_insert.val < p.val) {
                p_insert_pre = p_insert;
                p_insert = p_insert.next;
            }
            // 找到插入位置，插入元素
            p_insert_pre.next = p;
            p.next = p_insert;
            // 待插入元素变为下一个
            p = head_origin_pre.next;
        }
        
        return head_new_pre.next;
    }
}
```


