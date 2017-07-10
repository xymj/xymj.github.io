---
title: 25. Reverse Nodes in k-Group
date: 2017-07-10 22:01:14
tags: LeetCode
categories: LeetCode
---

# 25. Reverse Nodes in k-Group

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.
If the number of nodes is not a multiple of k then left - out nodes in the end should remain as it is.
You may not alter the values in the nodes, only nodes itself may be changed.
Only constant memory is allowed.

For example,

Given this linked list : 1->2->3->4->5
For k = 2, you should return : 2->1->4->3->5
For k = 3, you should return : 3->2->1->4->5

<!--more-->

## 题意：

　　给定一个链表，每次反转链表k个节点组（从左侧开始数，每k个节点为一个组合，组合内节点进行反转），并返回其修改后的链表。如果节点数不是k的倍数，那么最后的链表节点应该保持不变。不能更改节点中的值，只有链表节点自身，即通过指针的改变完成k个节点内的链表节点反转，并把每一反转组组合成一个新的链表。仅允许常量内存。

## 思路：

### 　方法一：

　　建立链表临时头结点，在不达到k的值时候进行表头插入，到达时临时头结点后移到链表尾部，然后在链表尾部完成下一组k个节点的头插法，**特别注意**当最后节点不足k个的时候，因为要保持原链表节点顺序，所以直接尾插法，完成最后一段链表组的插入。

```c++
ListNode* reverseKGroup(ListNode* head, int k) {
	if (head == NULL || head->next == NULL)
	{
		return head;
	}
	ListNode *p = head;
	int sum = 0;//记录链表中的总共节点数
	while (p)
	{
		sum++;
		p = p->next;
	}
	ListNode reverseHead(-1);
	ListNode *reversePointer = &reverseHead;
	//把大于k先头插，满足k组的话把临时头结点移动到分组最后一个再进行头插，依次循环直到sum<k，组成成组链表
	while (sum>=k)//当sum>=k的时候说明后面肯定有要进行反转的链表组
	{
		ListNode *tempH = NULL;
		//先进行头插法
		for (int i = 0; i < k; i++) {
			ListNode *pre = head;
			head = head->next;
			reversePointer->next = pre;
			pre->next = tempH;
			tempH = pre;
		}
		while (reversePointer->next)//把头插法的头结点移动到链表的尾部，便于下一次头插或者尾插
		{
			reversePointer = reversePointer->next;
		}
		sum -= k;
	}
	reversePointer->next = head;//最后一段尾插法
	return reverseHead.next;

}
```

### 　方法二：

　　思路大致和方法一相同，但是把链表的反转直接抽取出来，利用递归的思想，通过断开分组与整个链表的连接，得到一个独立的分组链表，然后直接对这一个分组进行反转，翻转后添加到临时头结点后面，每个独立的k个节点分组翻转后依次进行链表尾插，直到遍历完所有链表节点。

```c++
//递归实现链表反转
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
//对分组内节点进行反转
ListNode* reverseKGroup(ListNode* head, int k) {
	if (head == NULL || head->next == NULL)
	{
		return head;
	}
	ListNode *p = head;
	int sum = 0;
	while (p)
	{
		sum++;
		p = p->next;
	}
	ListNode reverseHead(-1);
	ListNode *reversePointer = &reverseHead;
	while (sum >= k)
	{
		ListNode *pre = head;
		for (int i=0;i<k-1;i++)
		{
			pre = pre->next;//找到断开分组的节点，便于递归反转
		}
		ListNode *tempHead = pre->next;//记录下一个分组的临时头结点
		pre->next = NULL;//断开分组
		auto everyHead = reverseList(head);
		head = tempHead;
		reversePointer->next = everyHead;
		while (reversePointer->next)
		{
			reversePointer = reversePointer->next;
		}
		sum -= k;
	}
	reversePointer->next = head;
	return reverseHead.next;
}
```

