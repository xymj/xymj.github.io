---
title: 21. Merge Two Sorted Lists
date: 2017-07-06 21:57:05
tags: LeetCode
categories: LeetCode
---

#  21. Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

## 题意：

　　合并两个排序链表为一个链表，合并后链表是有序链表。

<!--more-->

## 思路：

```c++
struct ListNode
{
	int val;
	ListNode *next;
	ListNode(int x) :val(x), next(NULL) {}

};
//切记:是合并两个不同的有序单链表为一个有序单链表
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
	ListNode *p1, *p2,*tempHead,*pc;
	pc =tempHead = l1;
	p1 = l1->next;
	p2 = l2->next;
	while (p1&&p2)
	{
		if (p1->val<=p2->val)
		{
			pc->next = p1;
			pc = pc->next;
			p1 = p1->next;
		}
		else
		{
			pc->next = p2;
			pc = pc->next;
			p2 = p2->next;
		}
	}
	pc->next = p1 ? p1 : p2;
	return tempHead;
}
```
---------------------------------------------------
#### 　Java Code：
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }


        ListNode head = new ListNode(-1);
        ListNode tmp = head;
        while (l1 != null && l2 != null) {
            int val1 = l1.val;
            int val2 = l2.val;
            if (val1 <= val2) {
                tmp.next = l1;
                l1 = l1.next;
            } else {
                tmp.next = l2;
                l2 = l2.next;
            }
            tmp = tmp.next;
        }

        if (l1 == null) {
            tmp.next = l2;
        } else {
            tmp.next = l1;
        }
        return head.next;
    }
}
```
