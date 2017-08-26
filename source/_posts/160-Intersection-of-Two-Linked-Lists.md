---
title: 160. Intersection of Two Linked Lists
date: 2017-08-26 11:22:30
tags: LeetCode
categories: LeetCode
---

Write a program to find the node at which the intersection of two singly linked lists begins.

```c++
For example, the following two linked lists :

    A:        a1 → a2
	                   ↘
	                     c1 → c2 → c3
	                   ↗
	B : b1 → b2 → b3

	begin to intersect at node c1.

```


Notes :
​	If the two linked lists have no intersection at all, return null.
​	The linked lists must retain their original structure after the function returns.
​	You may assume there are no cycles anywhere in the entire linked structure.
​	Your code should preferably run in O(n) time and use only O(1) memory.

<!--more-->

## 题意：

　　写一个程序来查找两个单链表相交的开始节点。
　　注意：
　　　　如果两个链表根本没有交集，返回null。
　　　　程序不能改变单链表原有的结构。
　　　　假设链表没有环的链接结构存在。
　　　　代码最好运行在O（n）时间复杂度，使用O（1）的内存空间。

## 思路：

### 　方法一：

　　如果两个链表相交，先用 countA=0, countB=0统计两个单链表的长度，因为如果二者有交点的话，如example中所示，二者的后半部分是相同的，成“Y”字形，所以只要减去长的单链表中和短单链表相差的元素个数，然后再同时让两个单链表向后移动，则当二者相同时就是二者的交点。 

```c++
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
	if (headA==NULL||headB==NULL)
	{
		return NULL;
	}
	ListNode *p=headA, *q=headB;
	int countA=0, countB=0;
	while (p !=NULL)
	{
		countA++;
		p = p->next;
	}
	while (q != NULL)
	{
		countB++;
		q = q->next;
	}
	int distances = countA >= countB ? countA - countB : countB - countA;
	while (distances)
	{
		if (countA >= countB)
			headA = headA->next;
		else
		{
			headB = headB->next;
		}
		distances--;
	}
	while (headA != NULL&&headB != NULL)
	{
		if (headA == headB)
		{
			return headA;
		}
		headA = headA->next;
		headB = headB->next;
	}
	return NULL;
}
```

### 　方法二：

　　不用整形变量值来记录哪个链表长，而是利用指针的移动差来重新设定除去长链表中的元素的头结点，然后再移动比较，当相等时就是要查找的第一个相同的节点。其实查找相同节点的方法是和上面一样的，但是找元素差不同。首先当A链表指针pA遍历到尾节点之后为空的话，就让其指向B链表头，如果B链表指针pB遍历到尾节点之后为空的话，就让其指向A链表头，这样交换，让二者相等相与的时候肯定是两个链表的开始节点。

```c++
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
	if (headA == NULL || headB == NULL)
		return NULL;
	ListNode *pA = headA;
	ListNode *pB = headB;
	while (pA!=NULL&&pB!=NULL&&pA!=pB)
	{
		pA = pA->next;
		pB = pB->next;
		if (pA==pB)
		{
			return pA;//这个条件也能控制把不想交的两个单链表结果返回为空
		}

		if (pA==NULL)//当为NULL交换指针指向
		{
			pA = headB;
		}
		if (pB==NULL)//当为NULL交换指针指向
		{
			pB = headA;
		}
	}
	return pA;//这个判断条件是为了满足第一个头结点开始就是相同节点的情况
}
```

