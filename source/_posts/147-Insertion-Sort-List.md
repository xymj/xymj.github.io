---
title: 147. Insertion Sort List
date: 2017-08-23 23:23:12
tags: LeetCode
categories: LeetCode
---

Sort a linked list using insertion sort.

## 题意：

　　使用插入排序对单链表进行排序。

<!--more-->

## 思路：

　　在遍历每个元素过程中，遍历已排序部分进行插入。首先建立一个临时的头结点，然后循环判断传入的链表有没有取到最后一个元素，此方法的一个思路清晰点就是，（1）分别用两个指针指向原链表，一个指向待比较的元素，一个指向待比较元素的下一个元素。（2）每次从头判断已经部分有序链表，用一个指针ListNode *cur = sortedHead;记录每次都是从有序链表的头结点开始比较，且cur指针指向有序链表中待比较元素的前面，若原值比所比较的值小，则在cur指针后插入，如果不小于则cur指针向后移动。

```c++
ListNode* insertionSortList(ListNode* head) {
	if (head == NULL || head->next == NULL)
	{
		return head;
	}
	ListNode *sortHead = new ListNode(-1);
	while (head!=NULL)
	{
		ListNode *temp = head->next;
		ListNode *cur = sortHead;
		while (cur->next!=NULL&&cur->next->val<head->val)
		{
			cur = cur->next;
		}
		head->next = cur->next;
		cur->next = head;
		head = temp;
	}
	return sortHead->next;
}
```

