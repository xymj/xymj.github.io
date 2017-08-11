---
title: 94. Binary Tree Inorder Traversal
date: 2017-08-11 21:59:15
tags: LeetCode
categories: LeetCode
---

# 94. Binary Tree Inorder Traversal

Given a binary tree, return the inorder traversal of its nodes' values.

For example :
Given binary tree[1, null, 2, 3],
　　1
　　　\
　　　2
　　　/
　　3
return[1, 3, 2]

<!--more-->

## 题意：

　　给定一个二叉树，实现二叉树的中序遍历的节点值。

## 思路：

### 　方法一：

　　非递归实现。

```c++
struct TreeNode {
	int val;
	TreeNode *left, *right;
	TreeNode(int x) :val(x), left(NULL), right(NULL) {}
};

//非递归实现
vector<int> inorderTraversal(TreeNode* root) {
	vector<int> vals;
	stack<TreeNode*> s;
	if (!root)
	{
		return vals;
	}
	TreeNode *temp = root;
	while (temp||s.empty())
	{
		if (temp)
		{
			s.push(temp);
			temp = temp->left;
		}
		else {
			temp = s.top();
			vals.push_back(temp->val);
			s.pop();
			temp = temp->right;
		}
	}
	return vals;
}
```

### 　方法二：

　　递归实现。

```c++
class Solution{
  vector<int> res;
 public:
  void inOrderTraversal(TreeNode *root) {
      if (root==NULL)
      {
          return;
      }
      inOrderTraversal(root->left);//递归调用，中序遍历左子树  
      res.push_back(root->val);//输出数据  
      inOrderTraversal(root->right); //递归调用，中序遍历右子树  
   }
};
```