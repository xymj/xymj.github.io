---
title: 107. Binary Tree Level Order Traversal II
date: 2017-08-14 23:06:17
tags: LeetCode
categories: LeetCode
---

# 107. Binary Tree Level Order Traversal II

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

```c++
For example:
Given binary tree [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
return its bottom-up level order traversal as:
[
  [15,7],
  [9,20],
  [3]
]
```

<!--more-->

## 题意：

　　给定一个二叉树，返回它的自底向上，从叶子节点到根节点的层序遍历的节点值。（从左到右，从叶到根逐层地遍历）。

## 思路：

### 　方法一：

　　按[102. Binary Tree Level Order Traversal](http://blog.taoaili999.cn/2017/08/12/102-Binary-Tree-Level-Order-Traversal/)的层序遍历思想，把重上到下顺序得到的数组反转，非递归实现：

```c++
class Solution {
public:
	vector<vector<int>> levelOrderBottom(TreeNode* root) {
		vector<vector<int>> res;
		if (!root)
		{
			return res;
		}
		vector<vector<int>> tempRes;
		queue<TreeNode*> q1;
		q1.push(root);
		while (!q1.empty())
		{
			queue<TreeNode*> q2;
			vector<int> nums;
			while (!q1.empty())
			{
				TreeNode *temp = q1.front();
				nums.push_back(temp->val);
				q1.pop();
				if (temp->left)
					q2.push(temp->left);
				if (temp->right)
					q2.push(temp->right);
			}
			tempRes.push_back(nums);
			q1 = q2;
		}
		//对层次遍历结果进行翻转  
		for (int i = tempRes.size()-1;i>=0;i--)
		{
			res.push_back(tempRes[i]);
		}
		return res;
	}
};
```

### 　方法二：

　　递归进行层次遍历，并将每个level对应于相应的vector。

```C++
class Solution {
public:
	vector<vector<int>> res;
	void getLevelNums(TreeNode* root, int level)
	{
		if (!root)
		{
			return;
		}
		if (level == res.size())
		{
			vector<int> num;
			res.push_back(num);
		}
		res[level].push_back(root->val);
		getLevelNums(root->left, level + 1);
		getLevelNums(root->right, level + 1);
	}
	vector<vector<int>> levelOrderBottom(TreeNode* root) {
		if (!root)
		{
			return res;
		}
		getLevelNums(root, 0);
		reverse(res.begin(), res.end());
		return res;
	}
};
```

