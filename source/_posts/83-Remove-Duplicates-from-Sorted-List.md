---
title: 83. Remove Duplicates from Sorted List
date: 2017-08-08 22:45:02
tags: LeetCode
categories: LeetCode
---

# 83. Remove Duplicates from Sorted List

Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,
Given 1->1->2, return 1->2.
Given 1->1->2->3->3, return 1->2->3.

<!--more-->

## 题意：

　　给一个排序链表，删除重复元素，使每个元素值出现一次，**注意**每个元素出现一次，并不是把重复的元素都删除掉，而是要保留一个元素在原链表中。

## 思路：

　　解法同82. Remove Duplicates from Sorted List II，但是在删除的过程中不能全部把相同的元素都删除掉，要留一个元素在链表中。这种能保留一个元素的有相对简单些，相等的话直接删除其后面的元素即可，不等后移链表指针。

```c++
ListNode* deleteDuplicates(ListNode* head) {
	if (head == NULL || head->next == NULL)
	{
		return head;
	}
	ListNode *p = head;
	while (p->next!=NULL) {
		if (p->val==p->next->val)
		{
			p->next = p->next->next;//如果相等，p->next直接指向 p->next->next元素，可以为空
		}
		else {
			p = p->next;
		}
	}
	return head;
}
```

