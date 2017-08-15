---
title: 109. Convert Sorted List to Binary Search Tree
date: 2017-08-15 22:22:40
tags: LeetCode
categories: LeetCode
---

# 109. Convert Sorted List to Binary Search Tree

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

## 题意：

　　给定一个单项升序链表，把其转换成二叉搜索树。

<!--more-->

## 思路：

　　思想同[108. Convert Sorted Array to Binary Search Tree](http://blog.taoaili999.cn/2017/08/14/108-Convert-Sorted-Array-to-Binary-Search-Tree/)找中点，构造根节点，但是此处是找链表的中点节点构造二叉搜索树的根节点，所以不能像数组一样可以直接控制中间节点的索引，应该通过链表中间节点，和最后一个节点，

### 方法一：

　　下面方法的巧妙之处就是把链表最后一个节点的下一个节点做为尾节点，即NULL，或者递归过程中的mid节点，这样可以避免查找中间节点的前一个节点，来断开与右子树对应的后半段链表。

```c++
class Solution {
public:
    TreeNode *sortedListToBST(ListNode *head)
    {
    	return sortedListToBST( head, NULL );//此处尾节点设为NULL
    }
private:
    TreeNode *sortedListToBST(ListNode *head, ListNode *tail)
    {
    	if( head == tail )
    		return NULL;
    	if( head->next == tail ) 
    	{	
    		TreeNode *root = new TreeNode( head->val );
    		return root;
    	}
    	ListNode *mid = head, *temp = head;
    	while( temp != tail && temp->next != tail )    // 寻找中间节点，不是判断！=NULL，而是判断!= tail，可以很好的控制来查找左右子树对应的链表的中间节点
    	{
    		mid = mid->next;
    		temp = temp->next->next;
    	}
    	TreeNode *root = new TreeNode( mid->val );
    	root->left = sortedListToBST( head, mid );//左子树对应链表的尾节点设为mid，相当于替换NULL，因为mid节点已经构造了树节点
    	root->right = sortedListToBST( mid->next, tail );
    	return root;
    }
}; 
```

### 方法二：

　　通过查找链表中间节点的过程中，返回链表中间节点的前一个节点，便于对应左右子树的子链表断开。

```c++
class Solution {
public:
    ListNode* getMidNode(ListNode* head)
    {
        if(!head || !head->next)
            return head;
        ListNode *fast = head;
        ListNode *slow = head;
        ListNode *slowPre = head;
        while(fast->next&&fast->next->next)
        {
            slowPre = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        return slowPre;
    }
    TreeNode* createBST(ListNode* head)
    {
        if(!head)
            return NULL;
        ListNode *midPre = getMidNode(head);
        ListNode *mid = midPre->next;
        if(mid)
        {
            TreeNode *root = new TreeNode(mid->val);
            midPre->next = NULL;
            TreeNode *left = createBST(head);
            TreeNode *right = createBST(mid->next);
            root->left = left;
            root->right = right;
            return root;
        }
        else 
            return new TreeNode(head->val);
    }
    TreeNode* sortedListToBST(ListNode* head) {
        if(!head)
            return NULL;
        return createBST(head);
    }
};
```

