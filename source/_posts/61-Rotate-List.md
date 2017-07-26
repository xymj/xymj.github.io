---
title: 61. Rotate List
date: 2017-07-26 22:56:56
tags: LeetCode
categories: LeetCode
---

# 61. Rotate List

Given a list, rotate the list to the right by k places, where k is non - negative.

For example :
Given 1->2->3->4->5->NULL and k = 2,
return 4->5->1->2->3->NULL.

<!--more-->

## 题意：

　　给定一个单链表，将单链表向右旋转k个位置，其中k是非负的，**注意**此处的向右旋转是循环的，旋转到投节点开始。其实就是单链表循环右移k次，每次移动一个结点。

## 思路：

　　因为可以右移就是循环移动，所以直接把单链表就是组成一个环，查找要断开的点即可。

```c++
struct ListNode
{
	int val;
	ListNode *next;
	ListNode(int x) :val(x), next(NULL) {}
};
ListNode* rotateRight(ListNode* head, int k) {
	if (head==NULL||head->next==NULL)
	{
		return head;
	}
	int length=0;
	ListNode *p = head;
	ListNode *pPre = NULL;
	while (p)
	{
		length++;
		pPre = p;
		p = p->next;
	}
	pPre->next = head; 
	int distances = length - k % length;//k是右侧移动元素个数，总长减去得到尾指针移动步数
	while (distances) {
		pPre = pPre->next;
		distances--;
	}
	ListNode *newHead = pPre->next;
	pPre->next = NULL;
	return newHead;
}
```

