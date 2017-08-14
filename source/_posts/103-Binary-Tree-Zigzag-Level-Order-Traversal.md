---
title: 103. Binary Tree Zigzag Level Order Traversal
date: 2017-08-14 22:03:10
tags: LeetCode
categories: LeetCode
---

# 103. Binary Tree Zigzag Level Order Traversal

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example :
Given binary tree[3, 9, 20, null, null, 15, 7],

```c++
 　 3
　 / \
  9  20
 / \
15  7
return its zigzag level order traversal as :
[
	[3],
	[20, 9],
	[15, 7]
]
```

<!--more-->

## 题意：

　　给定一个二叉树，返回其节点值的锯齿级顺序遍历。（从左到右，从右到左为下一层，在两层间交替从左到右或者从右到左的顺序）。

## 思路：

　　利用两个队列实现二叉树的层序遍历，遍历的结果值在把偶数层的值反转，假设根节点是第一层 ，掌握其中的层序遍历 方法以及[102. Binary Tree Level Order Traversal](http://blog.taoaili999.cn/2017/08/12/102-Binary-Tree-Level-Order-Traversal/)的层序遍历方法。

```c++
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
	vector<vector<int>> res;
	if (!root)
	{
		return res;
	}
	queue<TreeNode*> q1;
	q1.push(root);
	while (!q1.empty())
	{
		queue<TreeNode*> q2;
		vector<int> tempRes;
		while (!q1.empty())
		{
			TreeNode  *temp = q1.front();
			tempRes.push_back(temp->val);
			if (temp->left)
			{
				q2.push(temp->left);
			}
			if (temp->right)
			{
				q2.push(temp->right);
			}
			q1.pop();
		}
		q1 = q2;
		res.push_back(tempRes);
	}
	for (int i=1;i<res.size();i+=2)
	{
		reverse(res[i].begin(), res[i].end());
	}
	return res;
}
```

