---
title: 222. Count Complete Tree Nodes
date: 2017-08-31 21:59:31
tags: LeetCode
categories: LeetCode
---

Given a complete binary tree, count the number of nodes.

## 题意：

　　给定一棵完全二叉树，求树种节点的个数。

<!--more-->

## 思路：

　　如果用常规的解法一个个遍历，就是O(n)时间复杂度 ，不通过。因为是完全二叉树，满二叉树有一个性质是节点数等于2^h-1，h为高度，所以可以这样判断节点的左右高度是不是一样，如果是一样说明是满二叉树，就可以用刚才的公式，如果左右不相等就递归计算左右节点。

### 　方法一：

　　若设二叉树的深度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第 h 层所有的结点都连续集中在最左边，这就是完全二叉树。根据完全二叉树的这个性质，可以这样来求解完全二叉树中节点的个数。对于一个节点node，计算它最左端的节点到node的深度为leftDepth，计算它最右端的节点到node的深度是rightDepth；如果leftDepth和rightDepth相等，那么以node为根节点的树是一棵满二叉树，此时以node为根节点的树的节点个数是pow(2,leftDepth)-1；如果leftDepth和rightDepth不相等，递归求解node的左子树的节点数和右子树的节点数。

```c++
class Solution {
public:
	int depthLeft(TreeNode* root)
	{
		if (!root)
		{
			return 0;
		}
		int height = 0;
		while (root)
		{
			root = root->left;
			height++;
		}
		return height;
	}
	int depthRight(TreeNode* root)
	{
		if (!root)
		{
			return 0;
		}
		int height = 0;
		while (root)
		{
			root = root->right;
			height++;
		}
		return height;
	}
	int countNodes(TreeNode* root) {
		if (!root)
		{
			return 0;
		}
		int ld = depthLeft(root);
		int rd = depthRight(root);
		if (ld == rd)
		{
			int lnum = pow(2,ld) - 1;
			return lnum;
		}
		return countNodes(root->left) + countNodes(root->right) + 1;//最后加一加的是根节点
	}
};
```

### 　方法二：

　　也是比较左右子树的高度是否相等，如果左子树等于右子树的高度（都按左子树计算高度，因为完全二叉树的高度由左子树的高度确定的）则左子树肯定是完全二叉树，右子树再递归判读，如果左子树不等于右子树高度，右子树肯定是完全二叉树，左子树再递归判断。

```c++
class Solution {
public:
	int depth(TreeNode* root)
	{
		if (!root)
		{
			return 0;
		}
		int height = 0;
		while (root)
		{
			root = root->left;
			height++;
		}
		return height;
	}
	int countNodes(TreeNode* root) {
		if (!root)
		{
			return 0;
		}
		int ld = depth(root->left);
		int rd = depth(root->right);
		if (ld==rd)
		{
			return pow(2, ld) + countNodes(root->right);
		   /* int lnum = (2 << (ld - 1)) + countNodes(root->left);//当层数是0的时候左移公式出现问题
			return lnum;*/
		}
		return pow(2, rd) + countNodes(root->left);
	}
};
```

