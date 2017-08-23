---
title: 138. Copy List with Random Pointer
date: 2017-08-22 22:48:17
tags: LeetCode
categories: LeetCode
---

# 138. Copy List with Random Pointer

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

<!--more-->

## 题意：

　　给出了一个链表，每个节点包含一个额外的随机指针，它可以指向列表中的任意节点或NULL。
　　返回链表的深层拷贝。

## 思路：

　　如果是简单的copy List 的话，那么我们只需要从头到尾遍历下来，new出对应个数的Node，并把它们的连接关系设置好就可以了，但是这道题目中每个节点Node出现了Random属性，也就意味着可能当前结点Node所依赖的那个Random对应的结点还没有被创建出来。

　　主要做如下三步处理：

1. 在OldList中的每个结点后，插入一个CopyNode，这个结点的Random域为空，Next域指向OldList节点的下一个需要复制的节点。
2. 由于所有的CopyNode都已经创建出来了，我们就可以调整这些CopyNode真正的Random域的值了，oldItr->next->random = oldItr->random->next;。
3. 调整所有CopyNode的Next域的值，恢复OldList所有Node的Next的值到初始状态，**特别注意**，要恢复原链表

图解

![138. Copy List with Random Pointer_解法2](/images/138. Copy List with Random Pointer_解法2.jpg)

```c++
struct RandomListNode {
	int label;
	RandomListNode *next, *random;
	RandomListNode(int x) :label(x), next(NULL), random(NULL) {}
};
//1、构建新节点random指针：new1->random = old1->random->next, new2 -> random = NULL, new3 - random = NULL, new4->random = old4->random->next
//
//2、恢复原始链表以及构建新链表：例如old1->next = old1->next->next, new1->next = new1->next->next
RandomListNode *copyRandomList3(RandomListNode *head) {
	if (head == NULL)
	{
		return NULL;
	}
	RandomListNode *p = head;
  //1、构造新拷贝节点的random为空，新拷贝节点插入到旧节点之后的单链表
	while (p)
	{
		RandomListNode *pNext = p->next;
		RandomListNode *q = new RandomListNode(p->label);
		p->next = q;
		q->next = pNext;
		p = pNext;
	}
	//2、构建新节点random指针：new1->random = old1->random->next, new2 - random = NULL, new3 - random = NULL, new4->random = old4->random->next
	RandomListNode *oldItr = head;
	while (oldItr)//此处的条件判断很重要，用旧的头依次向后移动并判断
	{
		if (oldItr->random)
		{
			oldItr->next->random = oldItr->random->next;
		}
		oldItr = oldItr->next->next;//只要oldItr不为空，oldItr->next就不会为空，因为可定后面有复制的元素
	}
	//3、恢复原始链表以及构建新链表：例如old1->next = old1->next->next, new1->next = new1->next->next
	RandomListNode *newHead = head->next;
	RandomListNode *new2 = head->next;
	RandomListNode *old2 = head;
	while (new2->next!=NULL)
	{
		old2->next = old2->next->next;
		old2 = old2->next;
		new2->next = new2->next->next;
		new2 = new2->next;
	}
	old2->next = NULL;
	new2->next = NULL;
	return newHead;
}
```

