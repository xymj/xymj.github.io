---
title: 2. Add Two Numbers
date: 2017-06-22 23:10:13
tags: [LeetCode]
categories:
- LeetCode
---

# 2. Add Two Numbers

You are given two linked lists representing two non - negative numbers.The digits are stored in reverse order and each of their nodes contain a single digit.Add the two numbers and return it as a linked list.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output : 7 -> 0 -> 8

<!-- more -->

## 题意：

　　有两个链表作为输入，它们表示逆序的两个非负数。如下面的两个链表表示的是342和465这两个数。你需要计算它们的和并且用同样的方式逆序输出。如342 + 465 = 807, 你需要把结果表达为7 ->0 ->8
　　输入: (2 -> 4 -> 3) + (5 -> 6 -> 4)
　　输出:Output : 7 -> 0 -> 8



## 思路：

``` c++
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
	ListNode head(-1);
	ListNode *p = &head;
	int sum = 0;
    //两个有一个没到头，就继续
	while (l1!=NULL||l2!=NULL)
	{
		if (l1!=NULL)
		{
			sum += l1->val;
			l1 = l1->next;
		}
		if (l2!=NULL)
		{
			sum += l2->val;
			l2 = l2->next;
		}
		p->next = new ListNode(sum % 10);
		sum /= 10;
		p = p->next;
	}
    //记得处理最后的进位
	if (sum)
	{
		p->next = new ListNode(sum);
	}
	return head.next;
}
```

---------------------------------------------------
#### 　Java Code：
``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

        ListNode head = new ListNode(-1);
        ListNode tmp = head;
        int sum = 0;
        while(l1 != null || l2 != null) {
            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }

            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            tmp.next = new ListNode(sum % 10);
            sum /= 10;
            tmp = tmp.next;
        }

        if (sum > 0) {
            tmp.next = new ListNode(sum);
        }


        return head.next;
    }
}
```
