---
title: 203. Remove Linked List Elements
date: 2017-08-28 21:46:57
tags: LeetCode
categories: LeetCode
---

Remove all elements from a linked list of integers that have value val.

Example
Given : 1 -- > 2 -- > 6 -- > 3 -- > 4 -- > 5 -- > 6, val = 6
Return: 1 -- > 2 -- > 3 -- > 4 -- > 5

<!--more-->

## 题意：

　　删除链表中所有与给定值相等的元素节点。

## 思路：

　　思路很简单，一边遍历一边删除链表比较链表中元素值，如果等于目标值直接删除节点。**注意：**1、删除的节点在头结点的情况；2、删除节点后记得释放内存空间；

```c++
ListNode* removeElements(ListNode* head, int val) {
	if (head == NULL)
	{
		return NULL;
	}
	ListNode removeHead(-1);//建立临时头结点、便于删除链表第一个节点
	ListNode *tempHead = &removeHead;
	tempHead->next = head;
	ListNode *pre = head;
	while (pre != NULL)
	{
		if (pre->val == val)
		{
			tempHead->next = pre->next;
			pre->next = NULL;
			/*new     <-->  delete
			  new[]   <-->  delete[]
			  malloc  <-->  free
              C++中的new / delete是新实现的内存分配器，而malloc和free是C标准库实现的另一套内存分配器，他们走的是不同的算法，所以不能混用了.*/
			delete(pre);
			//free(pre);
			pre = tempHead->next;
		}
		else {
			pre = pre->next;
			tempHead = tempHead->next;
		}
	}
	return removeHead.next;
}
```

