---
title: 143. Reorder List
date: 2017-08-23 22:19:56
tags: LeetCode
categories: LeetCode
---

Given a singly linked list L : L0→L1→…→Ln - 1→Ln,
reorder it to : L0→Ln→L1→Ln - 1→L2→Ln - 2→…

You must do this in - place without altering the nodes' values.

For example,
Given{ 1,2,3,4 }, reorder it to{ 1,4,2,3 }.

<!--more-->

## 题意：

　　给定一个单链表正常顺序，然后重新组合链表节点顺序，规则如L0→Ln→L1→Ln - 1→L2→Ln - 2→…
　　必须在常量空间，切不能改变节点元素值的情况下解题。

## 思路：

### 　方法一：

　　非递归实现，先找到单链表的中点，然后断开，把后半部分逆转依次插入前半部分。

```c++
//递归实现链表的反转
ListNode* reverseList(ListNode *head) {
	if (head==NULL||head->next==NULL)
	{
		return head;
	}
	ListNode *p = head->next;
	ListNode *n = reverseList(p);
	head->next = NULL;
	p->next = head;
	return n;
}
void reorderList(ListNode* head) {
	ListNode *fast = head;
	ListNode *slow = head;
	while (fast!=NULL&&fast->next!=NULL)
	{
		fast = fast->next->next;
		slow = slow->next;
	}
	ListNode *lastHead = slow->next;
	slow->next = NULL;
	ListNode *n = reverseList(lastHead);
	while (n!=NULL)
	{
		ListNode *headPre = head->next;
		ListNode *nPre = n->next;
		head->next = n;
		n->next = headPre;
		head = headPre;
		n = nPre;
	}
}
```

### 　方法二：

　　递归实现，利用递归来执行head指针的前进和后退，当满足条件head == mid时递归结束，head往回移动。利用forward接收后半段的头结点，然后依次向后移动取节点插入到head的后面。

```c++
ListNode *reorderList(ListNode *head,int counts) {
	if (head == NULL )
	{
		return NULL;
	}
	if (counts==1)//count减后的值是head指针距离要断开指针的位置的大小
	{
		ListNode *temp = head->next;
		head->next = NULL;
		return temp;
	}
	else if (counts==2)
	{
		ListNode *temp = head->next->next;
		head->next->next = NULL;
		return temp;
	}
	else
	{
		ListNode *forward = reorderList(head->next, counts - 2);
		ListNode *temp = forward->next;
		forward->next = head->next;
		head->next = forward;
		return temp;	
	}
}
void reorderList(ListNode *head) {
	ListNode *slow = head;
	int counts = 0;//计算链表长
	while (slow)
	{
		slow = slow->next;
		counts++;
	}
	reorderList(head,counts);
}
```

