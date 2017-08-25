---
title: 148. Sort List
date: 2017-08-24 09:09:24
tags: LeetCode
categories: LeetCode
---

Sort a linked list in O(n log n) time using constant space complexity.

## 题意：

　　在 O(n log n)的时间复杂度内，在恒定的空间复杂度内，排序单链表。

<!--more-->

## 思路：

　　表面上看，能够有O(n lgn)时间复杂度的算法为，快速排序，堆排序，归并排序，三者的空间复杂度分别为O(1), O(N), O(N)，所以一开始，想着用快速排序的方法来解决，但是对于链表而言，快速排序没有归并排序时间复杂度低、稳定。
　　通常而言，也就是针对数组而言，归并排序的空间复杂度为O(N), 你需要开出O(N)的额外空间来容纳数组，来表示归并后的顺序。但是，对于链表而言，你可以省下这部分空间的开销，你只需要改变节点的next指针的指向，就可以表示新的归并后的顺序了，所以空间复杂度陡然降到了O(1)。
　　归纳一下，就是说，如果待排序的元素存储在数组中，那么快速排序相对归并排序就有两个原因更快。一是，可以很快地进行元素的读取(相对于链表，数组的元素是顺序摆放的，而链表的元素是随机摆放的)，数组的partion这步就比链表的partion这步快。二是，归并排序在merge阶段需要辅助数组，需要申请O(N)的空间，申请空间也是需要时间的。而快排不需要额外申请空间。如果待排序的元素存储在链表中，快排的优点就变成了缺点。归并排序于是就速度更优了。

### 　方法一：

　　链表的归并排序，获取单链表的中点，实现对单链表先划分，便于比较归并，然后进行两个单链表的合并，最后通过进行递归的归并排序，不断的划分单链表，然后再逐一进行合并。

```c++
//1、获取单链表的中点，实现对单链表先划分，便于后面的比较归并。
ListNode* getMid(ListNode *head) {
	if (head==NULL||head->next==NULL)
	{
		return head;
	}
	ListNode *fast = head;
	ListNode *slow = head;
	ListNode *preEnd = NULL;
	while (fast!=NULL&&fast->next!=NULL)
	{
		fast = fast->next->next;
		if (fast == NULL||fast->next == NULL)
		{
			preEnd = slow;
		}
		slow = slow->next;
	}
	preEnd->next = NULL;
	return slow;
}
//2、两个单链表的合并算法
ListNode* mergeTwoList(ListNode *l1, ListNode *l2) {
	ListNode newHead(-1);
	ListNode *newPointer = &newHead;
	while (l1&&l2)
	{
		if (l1->val<l2->val)
		{
			newPointer->next = l1;
			newPointer = newPointer->next;
			l1 = l1->next;
		}
		else
		{
			newPointer->next = l2;
			newPointer = newPointer->next;
			l2 = l2->next;
		}
	}
	while (l1)
	{
		newPointer->next = l1;
		newPointer = newPointer->next;
		l1 = l1->next;
	}
	while (l2)
	{
		newPointer->next = l2;
		newPointer = newPointer->next;
		l2 = l2->next;
	}
	return newHead.next;
}
//3、进行递归的归并排序，不断的划分单链表，然后再逐一进行合并。
ListNode* mergeSort(ListNode *head) {
	if (head==NULL||head->next==NULL)
	{
		return head;
	}
	ListNode *nextHead = getMid(head);
	ListNode *l1 = mergeSort(head);
	ListNode *l2 = mergeSort(nextHead);
	ListNode *res = mergeTwoList(l1, l2);
	return res;
}
```

### 　方法二：

　　链表的快速排序，和数组快速排序中相同，先把数据进行分块，这里利用两个指针p和q，这两个指针均往next方向移动，移动的过程中保持p之前的key都小于选定的key，p和q之间的key都大于选定的key，那么当q走到末尾的时候便完成了一次划分点的寻找。

```c++
//单链表快速排序：
//分为两步，首先找到链表的分割点(即左边都比他小，右边都比它大的点)，然后以分割点为左右在进行分割，即继续调用快速排序递归函数
//1、先把数据进行分块，经典划分法
ListNode* getPartion(ListNode *PBegin, ListNode *PEnd) {
	ListNode *p = PBegin;
	ListNode *q = PBegin->next;
	int valKey = PBegin->val;
	while (q!=PEnd)
	{
		if (q->val<valKey)
		{
			p = p->next;
			swap(p->val, q->val);
		}
		q = q->next;
	}
	swap(PBegin->val, p->val);//把标志点交换到中间，实现左边都是比他小的，右边都是比它大的。
	return p;
}
//2、进行递归形式的对每一个块再进行分块，最后得到有序的序列。
ListNode* Quicksort(ListNode *PBegin, ListNode *PEnd)
{
	if (PBegin!=PEnd)
	{
		ListNode *partion = getPartion(PBegin, PEnd);
		Quicksort(PBegin, partion);
		Quicksort(partion->next, PEnd);
	}
	return PBegin;
}

ListNode* sortList(ListNode* head) {
	return Quicksort(head，NULL);
}
```

