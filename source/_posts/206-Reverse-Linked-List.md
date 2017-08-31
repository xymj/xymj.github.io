---
title: 206. Reverse Linked List
date: 2017-08-28 22:47:11
tags: LeetCode
categories: LeetCode
---

Reverse a singly linked list.

Hint:
A linked list can be reversed either iteratively(迭代) or recursively(递归).Could you implement both ?

<!--more-->

## 题意：

　　反向单向链表：把整个链表反转，，最后一个节点是头结点，其实就是完成单链表的反转。
　　同时用迭代和递归实现。

## 思路：

### 　方法一：

　　利用迭代，非递归实现，利用三个指针，分别记录当前节点，当前节点的下一个节点，以及下一个节点的下一个节点，用于反转断开链表后从新获取后面未反转单链的头结点。

```c++
ListNode* reverseList(ListNode* head) {
	if (head == NULL || head->next == NULL)
		return head;
	ListNode *p = head;
	ListNode *tempfirst = head->next;
	ListNode *tempsecond = tempfirst->next;
	p->next = NULL;
	while (tempfirst != NULL)
	{
		tempfirst->next = p;
		//p->next = tempfirst;
		p = tempfirst;
		tempfirst = tempsecond;
		if (tempsecond != NULL)
			tempsecond = tempsecond->next;
	}
	return p;
}
```

### 　方法二：

　　递归实现，递归前指针p指向头结点的下一个节点，head指向头结点，一直向下递归，直到链表尾部，就像入栈操作一样，先入栈的后出栈，当递归返回上一层时，p在head后面，直接修改next指向，完成节点反转，然后继续返回上一层，直到递归栈为空，从而实现所有节点的反转。

```c++
ListNode* reverseList(ListNode* head) {
	if(head==NULL) 
		return NULL;
	if (head->next == NULL) 
		return head;
	ListNode *p = head->next;
	ListNode *n = reverseList(p);//n指针一直指向反转后的头结点
	head->next = NULL;
	p->next = head;
	return n;
}
```

