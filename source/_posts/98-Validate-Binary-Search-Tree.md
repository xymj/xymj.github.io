---
title: 98. Validate Binary Search Tree
date: 2017-08-12 11:33:03
tags: LeetCode
categories: LeetCode
---

# 98. Validate Binary Search Tree

Given a binary tree, determine if it is a valid binary search tree (BST).
Assume a BST is defined as follows:

　The left subtree of a node contains only nodes with keys less than the node's key.
　The right subtree of a node contains only nodes with keys greater than the node's key.
　Both the left and right subtrees must also be binary search trees.

Example 1:
　　　2
　　 /　 \
　　1　 3
Binary tree [2,1,3], return true.

Example 2:
　　　1
　　 / 　\
　  2       3
Binary tree [1,2,3], return false. 

<!--more-->

## 题意：

　　 给定一个二叉树，确定它是否是一个有效的二叉搜索树（BST）。 
 　　假设BST定义如下： 
 　　　　节点的左子树只包含小于该节点键的节点。 
 　　　　节点的右子树只包含大于节点键的键的节点。 
 　　　　根节点的左、右子树也必须是二叉搜索树。 

## 思路：

### 　方法一：

　　参照[99. Recover Binary Search Tree](http://blog.taoaili999.cn/2017/08/12/99-Recover-Binary-Search-Tree/)的思路，一边中序遍历二叉搜索树，一边进行判断，用一个指针记录前一个节点的值，因为中序遍历的二分查找树所得到的序列是递增序列，所以非递归实现如下：

```c++
class Solution {
public:
	bool isValidBST(TreeNode* root) {
		if (!root)
			return true;
		bool flag = true;
		TreeNode* pre = NULL;
		TreeNode* temp = root;
		stack<TreeNode*> s;
		//中序遍历的二分查找树所得到的序列是递增序列
		while (temp || !s.empty())
		{
			if (temp)
			{
				s.push(temp);
				temp = temp->left;
			}
			else
			{
				temp = s.top();
				s.pop();
				if (pre)
				{
					if (pre->val >= temp->val)
						flag = false;
				}
				pre = temp;
				temp = temp->right;
			}
		}
		return flag;
	}
};
```

### 　方法二：

　　递归先序遍历实现：

```c++
class Solution {
	TreeNode* pre;
	bool flag;
public:
	bool isValidBST(TreeNode* root) {
		if (!root)
			return true;
		flag = true;
		TreeNode* pre = NULL;
		inOrderFindNode(root);
		return flag;
	}
	void inOrderFindNode(TreeNode* root)
	{
		if (!root)
		{
			return;
		}
		inOrderFindNode(root->left);
		//cout << root->val << endl;
		if (pre)//当前驱结点不为空的时候再进行比较
		{
			if (pre->val >= root->val)
			{
				flag = false;
			}
		}
		pre = root;
		inOrderFindNode(root->right);
	}
};
```

