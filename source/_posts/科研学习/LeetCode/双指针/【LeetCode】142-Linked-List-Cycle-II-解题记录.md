---
title: 【LeetCode】142. Linked List Cycle II 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 双指针
  - 快慢指针
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: cc8b51d9
date: 2021-05-12 14:48:46
---


# 问题描述

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Notice that you should not modify the linked list.

## 测试样例

![](/images/【LeetCode】142-Linked-List-Cycle-II-解题记录/2021-05-12-14-36-50.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

![](images/【LeetCode】142-Linked-List-Cycle-II-解题记录/2021-05-12-14-37-57.png)

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

![](images/【LeetCode】142-Linked-List-Cycle-II-解题记录/2021-05-12-14-38-21.png)

```
Input: head = [1], pos = -1
Output: no cycle
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

对于链表找环路的问题，有一个通用的解法——快慢指针（Floyd 判圈法）。

1. 给定两个指针，分别命名为 `slow` 和 `fast`，起始位置在链表的开头。
1.  每次 `fast` 前进两步，`slow` 前进一步。
    - 如果 `fast` 可以走到尽头，那么说明没有环路；
    - 如果 `fast` 可以无限走下去，那么说明一定有环路，且一定存在一个时刻 `slow` 和 `fast` 相遇。
    
1. 当 `slow` 和 `fast` 第一次相遇时，将 `fast` 重新移动到链表开头，并让 `slow` 和 `fast` 每次都前进一步。
1. 当 `slow` 和 `fast` 第二次相遇时，相遇的节点即为环路的开始点。

**补充：**

1. 双指针
1. 快慢指针
1. 时间复杂度 `O(n)`


![](images/【LeetCode】142-Linked-List-Cycle-II-解题记录/2021-05-12-14-42-21.png)

假设：
- 链表头是 `X`，
- 环的第一个节点是 `Y`，
- 环的长度是 `L`，
- 各段的长度分别是 `a, b, c`，
- `slow` 和 `fast` 第一次的交点是 `Z`，

第一次相遇时，
  - `slow` 走过的距离为 `a + b`，
  - `fast` 走过的距离为 `a + b + c + b`。

因为 `fast` 的速度是 `slow` 的两倍，所以 `fast` 走的距离是 `slow` 的两倍，有 `2(a + b) = a + b + c + b`，可以得到 `a = c`。

此时，`slow` 位于 `Z`，令 `fast` 位于首节点 `X`，两指针每次均前进一步，由于 `a = c`，则两指针必相遇于 `Y` 点。

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
    public ListNode detectCycle(ListNode head) {
        // 快慢指针
        ListNode p_slow = head, p_fast = head;
        // 尝试是否相遇，相遇则有环
        do {
            // 无环
            if(p_fast == null || p_fast.next == null)     return null;
            // 有环,快指针前进两步，慢指针前进一步
            p_fast = p_fast.next.next;
            p_slow = p_slow.next;
        } while(p_fast != p_slow);
        // 若无环，则在以上循环中已返回
        // p_fast 从 head 开始，p_fast 和 p_slow 每次均前进一步，相遇点即为环交点
        p_fast = head;
        while(p_fast != p_slow) {
            p_fast = p_fast.next;
            p_slow = p_slow.next;
        }
        return p_fast;
    }
}
```
