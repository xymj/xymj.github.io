---
title: 86. Partition List
date: 2017-08-09 21:15:03
tags: LeetCode
categories: LeetCode
---

# 86. Partition List

Given a linked list and a value x, partition(划分) it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve（保持） the original（原始的） relative（相对的） order of the nodes in each of the two partitions.

For example,
Given 1->4->3->2->5->2 and x = 3,
return 1->2->2->4->3->5.

<!--more-->

## 题意：

　　给定一个链表和一个值x，通过x划分链表中所有节点，使链表前面的节点小于X，后面的节点大于或等于x，要保持原始的相对在一个分区的节点顺序。

## 思路：

　　首先创建两个单链表的头结点，分别用来指向小于x的单链表，和大于等于x的单链表，最后把后者连接到前者的单链表上，这样原单链表的相对顺序也不会改变。   **注意--**第二个链表的末尾要置为NULL。

```c++
ListNode* partition(ListNode* head, int x) {
	if (head==NULL)
	{
		return NULL;
	}
	ListNode *p = head;
	ListNode tempHead1(-1);
	ListNode *temp1 = &tempHead1;
	ListNode tempHead2(-1);
	ListNode *temp2 = &tempHead2;
	while (p!=NULL)
	{
		if (p->val<x)
		{
			temp1->next = p;
			temp1 = temp1->next;
		}
		if (p->val >= x)
		{
			temp2->next = p;
			temp2 = temp2->next;
		}
		p = p->next;
	}
	temp2->next = NULL;//不置为空的话有可能还指向原链表中的元素，当和第一个链表连接起来的时候回造成环的出现，导致错误
	temp1->next = tempHead2.next;
	tempHead2.next = NULL;
	//delete(&tempHead2);
	return tempHead1.next;
}
```

