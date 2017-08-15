---
title: 111. Minimum Depth of Binary Tree
date: 2017-08-15 22:56:25
tags: LeetCode
categories: LeetCode
---

# 111. Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

<!--more-->

## 题意：

　　给定一个二叉树，找出它的最小深度。
　　最小深度是从根节点到最近叶节点的最短路径上的节点数。

## 思路：

　　同样采用递归的思想，同求树高不同之处在于：找最小距离要注意(l<r)? l+1:r+1的区别应用，当根节点为空，返回0，当根节点为唯一的二叉树节点时，返回1；否则，求解并返回 最小(非空，保证最终到达叶节点)左右子树深度 + 1；

```c++
class Solution {
public:
	int minDepth(TreeNode* root) {
		if (!root)
			return 0;
		int ld = minDepth(root->left);
		int rd = minDepth(root->right);
		if (ld == 0)//肯定最低的子树先到叶子节点，先等于0
			return rd + 1;
		if (rd == 0)
			return ld + 1;
		return (ld>rd ? rd : ld) + 1;//返回最小高度加1
	}
};
```



