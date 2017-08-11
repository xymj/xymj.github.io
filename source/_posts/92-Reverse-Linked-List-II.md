---
title: 92. Reverse Linked List II
date: 2017-08-09 23:28:40
tags: LeetCode
categories: LeetCode
---

# 92. Reverse Linked List II

Reverse a linked list from position m to n.Do it in - place and in one - pass.

For example :
Given 1->2->3->4->5->NULL, m = 2 and n = 4,
return 1->4->3->2->5->NULL.

Note :
Given m, n satisfy the following condition :
1 ≤ m ≤ n ≤ length of list.

<!--more-->

## 题意：

　　将一个链表从位置m到n进行反转，在原内存空间上操作链表，完成链表m到n反转。
　　**注意：** 给定的m，n满足以下条件： 1≤m≤n≤列表的长度。 

## 思路：

### 　方法一：

　　直接在查找链表的过程中进行反转链表的指定区域。

```c++
ListNode* reverseBetween(ListNode* head, int m, int n) {
	if (head == NULL || head->next == NULL)
	{
		return head;
	}
	if (m == n)
	{
		return head;
	}
	ListNode *endHead = NULL;
	ListNode *vPre = NULL;
	ListNode *v = head;
	ListNode *endReverse = NULL;
	int tempM = m;
	while ((tempM -1))
	{
		endHead = v;
		v = v->next;
		tempM--;
	}
	endReverse = v;
	vPre = v;
	v = v->next;
	for (int i = m+1;i<=n;i++)
	{
		ListNode *vNext = v->next;
		v->next = vPre;
		vPre = v;
		v = vNext;
	}
	endReverse->next = v;
	if (endHead)
	{
		endHead->next = vPre;
	}
	else
	{
		head = vPre;
	}
	return head;
}
```

###  方法二：

　　分为三段，把中间的一段进行反转,利用递归进行反转单链表的实现函数，效率不太高。

```c++
ListNode* reverseList(ListNode* head)
{
	if (head==NULL||head->next==NULL)
	{
		return head;
	}
	ListNode *p = head->next;
	ListNode *q = reverseList(p);
	head->next = NULL;
	p->next = head;
	return q;
}
//分为三段，把中间的一段进行反转
ListNode* reverseBetween(ListNode* head, int m, int n) {
	if (head == NULL || head->next == NULL)
	{
		return head;
	}
	if (m==n)
	{
		return head;
	}
	ListNode *mPointer = head;
	ListNode *nPointer = head;
	ListNode *mPre = NULL;
	ListNode *nLater = NULL;
	while ((m-1))
	{
		mPre = mPointer;
		mPointer = mPointer->next;
		m--;
	}
	while ((n - 1))
	{
		nPointer = nPointer->next;
		n--;
	}
	if (nPointer->next!=NULL)
	{
		nLater = nPointer->next;
		nPointer->next = NULL;//一定要设为NULL
	}
	ListNode *mHead = reverseList(mPointer);
	if (mPre!= NULL)
	{
		mPre->next = mHead;
	}
	else
	{
		head = nPointer;
	}
	while (mHead->next!=NULL//找反转后的中间段的尾节点
	{
		mHead = mHead->next;
	}
	mHead->next = nLater;
	return head;
}
```