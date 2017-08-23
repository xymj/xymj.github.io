---
title: 124. Binary Tree Maximum Path Sum
date: 2017-08-21 22:20:45
tags: LeetCode
categories: LeetCode
---

# 124. Binary Tree Maximum Path Sum

Given a binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path does not need to go through the root.

```c++
For example:
Given the below binary tree,
      1
     / \
    2   3
Return 6. 
```

<!--more-->

## 题意：

　　给出一个二叉树，找出最大路径的总和。
　　对于这个问题，路径被定义为从一个起始节点到树中的任何节点沿着父子关系连接的任意序列。**注意：路径不需要经过根。**

## 思路：

　　主要是理解题意，就是查找从一个节点到其左右子树中最大路径值得和。
　　需要考虑以上两种情况：
　　　　1 左子树或者右子树中存有最大路径和不能和根节点形成一个路径
　　　　2 左子树 右子树 和根节点形成最大路径

```c++
class Solution {
public:
	int maxPathSum(TreeNode* root) {
		int maxSum = INT_MIN;
		getMaxPath(root, maxSum);
		return maxSum;
	}
	int getMaxPath(TreeNode* root, int &maxSum) {
		if (!root)
		{
			return 0;
		}
         //左子树最大路径值(路径特点：左右节点只能选一个)
		int leftSum = getMaxPath(root->left, maxSum);
         //右子树最大路径值(路径特点：左右节点只能选一个)
		int rightSum = getMaxPath(root->right, maxSum);
         //以root节点的双侧路径((root节点以及左右子树))
		int curSum = root->val;
		if (leftSum>0)
		{
			curSum += leftSum;
		}
		if (rightSum>0)
		{
			curSum += rightSum;
		}
		maxSum = max(curSum, maxSum);
        // 以root节点的单侧路径(root节点以及左右子树的一个)，求其中最大值
		return max(root->val, max(root->val + leftSum, root->val + rightSum));
	}
};
```

