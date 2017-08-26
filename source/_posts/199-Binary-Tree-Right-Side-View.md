---
title: 199. Binary Tree Right Side View
date: 2017-08-26 17:57:24
tags: LeetCode
categories: LeetCode
---

Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

```c++
For example:
Given the following binary tree,

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
You should return [1, 3, 4].
```

<!--more-->

## 题意：

　　给定一棵二叉树，返回从右边看这棵二叉树所看到的节点序列（从上到下）。

## 思路：

　　层次遍历法，遍历到每层最后一个节点时，把其放到结果集中。

```c++
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
class Solution {
public:
	vector<int> rightSideView(TreeNode* root) {
		vector<int> res;
		if (!root)
			return res;
		queue<TreeNode*> que1;
		que1.push(root);
		while (!que1.empty())
		{
			queue<TreeNode*> que2;
			while (!que1.empty()) {
				TreeNode* temp = que1.front();
				que1.pop();
				if (temp->left)
				{
					que2.push(temp->left);
				}
				if (temp->right)
				{
					que2.push(temp->right);
				}
				if (que1.empty())
				{
					res.push_back(temp->val);//取最右边元素放入结果集
				}
			}
			que1 = que2;
		}
		return res;
	}
};
```



