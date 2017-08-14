---
title: 104. Maximum Depth of Binary Tree
date: 2017-08-14 22:20:53
tags: LeetCode
categories: LeetCode
---

# 104. Maximum Depth of Binary Tree

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

<!--more-->

## 题意：

　　给定一个二叉树，求其最大深度。
　　最大深度是从根节点到最远叶节点的最长路径上的节点数。

## 思路：

### 　方法一：

　　递归实现，利用二叉树的递归求二叉树树高：

```c++
class Solution {
public:
	int maxDepth(TreeNode* root) {
		if (!root)
		{
			return 0;
		}
		else {
			int dl = maxDepth(root->left);
			int dr = maxDepth(root->right);
			return (dl > dr ? dl : dr) + 1;
		}
	}
};
```

### 　方法二：

　　利用二叉树的非递归层序遍历求树高：

```c++
class Solution {
public:
	int maxDepth(TreeNode* root) {
		if (!root)
		{
			return 0;
		}
		int depth = 0;
		queue<TreeNode*> q1;
		q1.push(root);
		while (!q1.empty())
		{
			queue<TreeNode*> q2;
			while (!q1.empty())
			{
				TreeNode *tp = q1.front();
				if (tp->left)
				{
					q2.push(tp->left);
				}
				if (tp->right)
				{
					q2.push(tp->right);
				}
				q1.pop();
			}
			depth++;
			q1 = q2;
		}
		return depth;
	}
};
```

