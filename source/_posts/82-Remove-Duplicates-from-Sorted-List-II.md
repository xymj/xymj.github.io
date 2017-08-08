---
title: 82. Remove Duplicates from Sorted List II
date: 2017-08-08 22:22:32
tags: LeetCode
categories: LeetCode
---

# 82. Remove Duplicates from Sorted List II

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

For example,
Given 1->2->3->3->4->4->5, return 1->2->5.
Given 1->1->1->2->3, return 2->3.

<!--more-->

## 题意：

　　给一个有序链表，在原链表中删除重复的元素，留下单一元素。

## 思路：

### 　方法一：

　　遍历链表的过程中删除重复元素，此法是找到相同的段，然后一起删除，也就是说通过计数数组中相同元素的个数，然后直接删除一整段。

```c++
struct ListNode
{
	int val;
	ListNode *next;
	ListNode(int x) :val(x), next(NULL) {}
};

ListNode* deleteDuplicates(ListNode* head) {
	if (head==NULL||head->next==NULL)
	{
		return head;
	}
	ListNode deleteHead(-1);//建立临时头结点
	ListNode *pPre = &deleteHead;
	pPre->next = head;
	ListNode *p = deleteHead.next;
	int counts = 1;//用于记录一段相同元素的个数
	while (p->next!=NULL)
	{
		ListNode *pNext = p->next;
		if (p->val==pNext->val)
		{
			counts++;
			p = p->next;
		}
		else
		{
			if (counts>1)//大于一有相同元素，直接通过pPre和pNext指针删除相同断
			{
				ListNode *tempHead = NULL;
				tempHead = pPre->next;
				pPre->next = pNext;
				p->next = NULL;
				delete(tempHead);
				p = pNext;
				counts = 1;
			}
			else
			{
				pPre = pPre->next;
				p = p->next;
			}
		}
	}
	if (counts>1)
	{
		pPre->next = NULL;
	}
	return deleteHead.next;
}
```

### 　方法二：

　　遍历链表的过程中删除重复元素，此法是找到一个相等的就删除一个，直到不等。

```c++
ListNode* deleteDuplicates(ListNode* head) {
	if (head == NULL || head->next == NULL)
	{
		return head;
	}
	ListNode deleteHead(-1);
	ListNode *pPre = &deleteHead;
	pPre->next = head;
	ListNode *p = deleteHead.next;
	while (p!=NULL&&p->next!=NULL)
	{
		ListNode *pNext = p->next;
		if (p->val==pNext->val)
		{
			int val = p->val;
			while (p!=NULL&&p->val == val)
			{
				pPre->next = pNext;
				p = pNext;
				if (pNext!=NULL)
				{
					pNext = pNext->next;
				}	
			}
		}
		else
		{
			pPre = pPre->next;
			p = p->next;
		}
	}
	return deleteHead.next;
}
```

