---
title: 142. Linked List Cycle II
date: 2017-08-23 21:20:01
tags: LeetCode
categories: LeetCode
---

Given a linked list, return the node where the cycle begins.If there is no cycle, return null.

Note: Do not modify the linked list.

<!--more-->

## 题意：

　　给定一个链表，如果存在环，返回环开始的链表节点，如果不存在环返回空。**注意：**不要修改链表。

## 思路：

　　 首先判断是否存在环，思路同[141. Linked List Cycle](http://blog.taoaili999.cn/2017/08/22/141-Linked-List-Cycle/)，对单恋表快慢指针相遇后，相遇点到环起始点，以及单链表头到环起始点的距离进行总结，可以得到如下结论——相遇点到环起始点的距离和链表头到环起始点的距离相同（不带链表头结点）；相遇点到环起始点的距离和链表头到环起始点的距离相差1（带链表头结点，因为ListNode head(-1);会增加1个距离）。

```c++
切记此结论是从相遇点起算起到环起始点的距离，以及快指针是慢指针的2倍移动速度。
切记要是想用相等距离，一定从带链表真实值的第一个节点作为头结点。
Example:
  图中：        X     a      Y    b    Z
                -------------|---------| 
                             |         |
							 |---------| 
                              c
  X是链表的起点。   
  Y是环的起点。
  Z是fast和slow首次相遇的地方（二者同时从X出发，slow每次移动一步，fast每次移动两步）。
  a, b, c分别表示XY（蓝色）, YZ（红色）, ZY（绿色）的长度。

  当fast和slow在Z点首次相遇时：

  fast移动的距离是：a + b + c + b
  slow移动的距离是：a + b
  因为fast的移动速度是slow的两倍，所以：
  (a + b + c + d) == 2 * (a + b)

  由此可以推出：
  a == c

  我们需要用上面的推论来寻找环的起点（Y）。
  当fast和slow首次相遇时，我们就到了Z点。
  由于a == c，也就是X到Y 与 Z到Y的距离相等。
  因此，如果我们让指针p和q分别从X和Z出发，并且每次都移动一步，当它们相遇时，恰好就是环的起点Y。
```
```c++
ListNode* detectCycle(ListNode *head) {
	if (head == NULL)
	{
		return NULL;
	}
	ListNode *fast, *slow;
	fast = slow = head;
	while (fast!=NULL&&fast->next!=NULL)
	{
		fast = fast->next->next;
		slow = slow->next;
		while (fast==slow)
		{
			auto tempHead1 = head;
			auto  tempHead2 = slow;
			while (tempHead1!=tempHead2)
			{
				tempHead1 = tempHead1->next;
				tempHead2 = tempHead2->next;
			}
			return tempHead1;	
		}
	}
	return NULL;
}

```

