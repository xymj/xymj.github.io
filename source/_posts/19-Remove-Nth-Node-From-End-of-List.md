---
title: 19. Remove Nth Node From End of List
date: 2017-07-05 21:38:48
tags: LeetCode
categories: LeetCode
---

# 19. Remove Nth Node From End of List

Given a linked list, remove the nth node from the end of list and return its head.

For example:
Given linked list : 1->2->3->4->5, and n = 2.
After removing the second node from the end, the linked list becomes 1->2->3->5.

Note :
​	Given n will always be valid.
​	Try to do this in one pass.

<!-- more-->

## 题意：

　　给定一个链表，移除链表从最后一个节点开始的第n个节点，n值肯定是有效的，尝试一次遍历单链表实现。

## 思路：

　　先有一个指针pre按照给定的倒数第n个的值向后移动，当n==0时，last指针开始从头结点开始向后移动，，然后pre和last指针同时移动，当pre为空时，则last指针指向的是所删除元素的前一个元素，然后利用last删除元素，返回链表头结点。
　　**特别注意**n所指的元素刚刚好是链表的第一个元素的时候。

```c++
struct ListNode
{
	int val;
	ListNode *next;
	ListNode(int x) :val(x), next(NULL) {}
};

ListNode* removeNthFromEnd(ListNode* head, int n) {
	if (head == NULL) return NULL;
	ListNode *pre =  head, *last = head;
	while (pre !=NULL)
	{
		while (n)
		{
			pre = pre->next;
			n--;
		}
        //注意此判断条件，因为n所指向的元素刚好是链表第一个元素的时候，按照下面的方法就会出错，因为n为第一个元素，则pre刚好移动到链表尾后空指针处，pre = pre->next;就会出错，所以要单独设立条件考虑。
		if (!pre)
		{
			return head->next;
		}
		pre = pre->next;
		if (pre!=NULL)
		{
			last = last->next;
		}
	}
	ListNode *tempNPoint = last->next;
	last->next = tempNPoint->next;
	tempNPoint->next = NULL;
	delete(tempNPoint);
	return head;
}

```

