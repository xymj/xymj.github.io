---
title: 110. Balanced Binary Tree
date: 2017-08-15 22:36:38
tags: LeetCode
categories: LeetCode
---

# 110. Balanced Binary Tree

Given a binary tree, determine if it is height - balanced.

For this problem, a height - balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

<!--more-->

## 题意：

　　给定一棵二叉树，确定它是否是高度平衡树。
　　一个高度非平衡二叉树定义为一个二叉树的深度的两个子树节点不会相差超过1。

## 思路：

　　 主要思想就是递归求树高，然后遍历所有树节点求树高，根据返回的左右子树的高度去判断子树高度相差是否大于1，大于1则不是高度平衡树。有两种方式判读，第一种是每遍历一个节点都求一回树高，在循环遍历里面比较左右子树高度，直到叶子节点，这种时间复杂度相对高些；第二种在递归求树高的递归函数中进行比较左右子树高度，一旦出现高度差大于1，直接返回，减少递归和遍历次数，时间复杂度低。

### 　方法一：

　　上述第一种思路：

```c++
class Solution {
public:
	bool isBalanced(TreeNode* root) {
		if (!root)
		{
			return true;
		}
		int leftDepth = depth(root->left);
		int rigthDepth = depth(root->right);
		if (abs(leftDepth - rigthDepth) > 1)//递归遍历每一个节点的过程中判断是否平衡
		{
			return false;
		}
		return isBalanced(root->left) && isBalanced(root->right);//递归遍历所有节点
	}
	int depth(TreeNode* root)
	{
		if (!root)
		{
			return 0;
		}
		int l = depth(root->left);
		int r = depth(root->right);
		return (l > r ? l : r) + 1;
	}
};
```

### 　方法二：

　　上述第二种思路：

```c++
//不在isBalanced()函数里面递归的判断是否平衡，在递归求树的高度的时候就进行判断
class Solution {
public:
	bool isBalanced(TreeNode* root) {
		if (!root)
		{
			return true;
		}
		int leftDepth = depth(root->left);//求根节点的左子树树高
		int rigthDepth = depth(root->right);//求根节点的右子树树高
		//如果左右子树有不平衡的地方则返回-1
		if (leftDepth<0 || rigthDepth<0 || abs(leftDepth-rigthDepth)>1)//最后的绝对值与1相比是比较根节点的左右子树
		{
			return false;
		}
		return true;
		
	}
	int depth(TreeNode* root)
	{
		if (!root)
		{
			return 0;
		}
		int l = depth(root->left);
		int r = depth(root->right);
		if (l<0 || r<0 || abs(l-r)>1)//递归求树高的过程中进行判断是否平衡
		{
			return -1;
		}
		else {
			return (l > r ? l : r) + 1;
		}
	}
};
```

