---
title: 141. Linked List Cycle
date: 2017-08-22 23:28:46
tags: LeetCode
categories: LeetCode
---

# 141. Linked List Cycle

Given a linked list, determine if it has a cycle in it.

Follow up :
Can you solve it without using extra space ?

<!--more-->

## 题意：

　　给定一个链表，判断此链表是否有环，不使用额外内存空间。

## 思路：

　　定义一个快指针pre，定义一个慢指针last，遍历给定链表，快指针每次前进两步pre = pre->next->next，慢指针每次前进一步 last = last->next，当快慢指针相与时，说明链表存在环，反之不存在。

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
    bool hasCycle(ListNode *head) {
        if(head==NULL)
            return false;
        ListNode *pre = head, *last = head;
    	while (pre!=NULL&&pre->next!=NULL)
    	{
    		pre = pre->next->next;
    		last = last->next;
    		if (last == pre)
    		{
    			return true;
    		}
    	}
    	return false;
    }
};
```

