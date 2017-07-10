---
title: 24. Swap Nodes in Pairs
date: 2017-07-09 22:29:20
tags: LeetCode
categories: LeetCode
---

# 24. Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head.

For example,
Given 1->2->3->4, you should return the list as 2->1->4->3.

Your algorithm should use only constant space.You may not modify the values in the list, only nodes itself can be changed.

<!--more-->

## 题意：

　　给一个单向链表，交换两个相邻节点，并且返回链表头结点。算法要求线性空间复杂度，并且不能只是交换节点的变量值，而是交换两个节点本身。

## 思路：

### 　方法一：

　　主要是构造临时头结点，因为LeetCode中构造的链表是不带头结点的，自己构造头结点可以完成一边遍历链表，一边交换两个相邻节点，关键就是利用三个指针分别记录三个节点，temp记录要反转的两个节点的前一个节点，p指向要反转的前一个节点，q指向后一个，这样一边往前遍历，一边改变next指针的指向。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode tempHead(-1);
        tempHead.next = head;
        if (head==NULL||head->next==NULL)
    	{
    		return head;
    	}
    	ListNode *p, *q,*temp;
    	temp = &tempHead;
    	p = tempHead.next;
    	q = p->next;
    	while (q != NULL&&p!=NULL)
    	{
    		p->next = q->next;
    		q->next = p;
    		temp->next = q;
    
    		temp = p;
    		p = p->next;
    		if(p)
    			q = p->next;
    	}
    	return tempHead.next;
    }
};
```

### 　方法二：

　　主要是利用递归，上层指向节点值得指针记录的都是递归前链表节点的位置，就是前一个节点位置指针，进入下一层函数的返回值是后面已经交换的两个链表节点的首节点，或者是最后一个节点节点指针。然后改变next指针，完成相邻节点的交换。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
      if (head == NULL) return NULL;
      if (head->next == NULL) return head;

      ListNode* temp = head->next;
      head->next = swapPairs(temp->next);
      temp->next = head;
      return temp;
    }
};
```

