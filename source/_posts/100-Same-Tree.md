---
title: 100. Same Tree
date: 2017-08-12 12:33:47
tags: LeetCode
categories: LeetCode
---

# 100. Same Tree

Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

<!--more-->

## 题意：

　　给定两个二叉树，编写一个函数检查它们是否相等。二叉树相同要求他们的树结构相同，节点中的值也相同。

## 思路：

### 　方法一：

　　先遍历的变形，同时先序遍历两个树结构，非递归实现：

```c++
class Solution {
public:
	bool isSameTree(TreeNode* p, TreeNode* q) {
		stack<TreeNode*> ps;
		stack<TreeNode*> qs;
		TreeNode *temp = p;
		TreeNode *temq = q;
		if ((temp == NULL&&temq != NULL) || (temp != NULL&&temq == NULL))
			return false;
		while ((temp || !ps.empty())&&(temq || !qs.empty()))
		{
			if ((temp == NULL&&temq != NULL) || (temp != NULL&&temq == NULL))
				return false;
			if (temp&&temq)
			{
				if (temp->val!=temq->val)
				{
					return false;
				}
				ps.push(temp);
				qs.push(temq);
				temp = temp->left;
				temq = temq->left;
			}
			else
			{
				temp = ps.top()->right;
				ps.pop();
				temq = qs.top()->right;
				qs.pop();
			}
		}
		return true;
	}
};
```

### 　方法二：

　　先遍历的变形，同时先序遍历两个树结构，递归实现：

```c++
class Solution {
public:
    bool isSameTree(TreeNode *p, TreeNode *q) {
        if(!p && !q)
            return true;
        else if(!p && q)
            return false;
        else if(p && !q)
            return false;
        else
        {
            if(p->val != q->val)
                return false;
            else
                return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
        }
    }
};
```

