---
title: 129. Sum Root to Leaf Numbers
date: 2017-08-21 22:59:45
tags: LeetCode
categories: LeetCode
---

# 129. Sum Root to Leaf Numbers

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.
An example is the root-to-leaf path 1->2->3 which represents the number 123.
Find the total sum of all root-to-leaf numbers.

```c++
For example,
    1
   / \
  2   3
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Return the sum = 12 + 13 = 25. 
```

<!--more-->

## 题意：

　　给定一个二叉树只包含数字0-9，根节点到每个叶节点路径可以表示一个数。例如根节点到叶节点路径路1 -> 2 -> 3代表数字123。求所有路径组合成的数的总和。

## 思路：

### 　方法一：

　　层序遍历的时候只要不到叶节点，就把本节点的值乘10加到左右孩子节点上，这样每遍历一层，和就会相加一层，当是叶节点时，从根节点到本叶节点的数字也就求出来了，加到总和sum中，按队列，广度优先遍历，非递归实现。

```c++
class Solution {
public:
	int sumNumbers(TreeNode* root) {
		int sum = 0;
		if (!root)
		{
			return sum;
		}
		queue<TreeNode*> q;
		q.push(root);
		while (!q.empty())
		{
			int levelSize = q.size();
			while (levelSize > 0)//控制每层，进行层序遍历
			{
				TreeNode* temp = q.front();
				q.pop();
				if (!temp->left&&!temp->right)//到达叶节点，把到叶节点的路径和加入到总sum中
				{
					sum += temp->val;
				}
				if (temp->left)
				{
					temp->left->val = temp->val * 10 + temp->left->val;//把本层节点值*10加入到下层左孩子节点中
					q.push(temp->left);
				}
				if (temp->right)
				{
					temp->right->val = temp->val * 10 + temp->right->val;//把本层节点值*10加入到下层右孩子节点中
					q.push(temp->right);
				}
				levelSize--;
			}
		}
		return sum ;
	}
};
```

### 　方法二：

　　递归实现，节点可能出现的情况:
　　　　(1)左孩子或者右孩子两个都为NULL(叶节点)，此时递归返回。
　　　　(2)左孩子或者右孩子某一个为NULL，此时继续推进下一层调用，只是在下一层中，会发现有一个为NULL节点，走到NULL节点当然应该返回了。
　　　　(3)左孩子或者右孩子都不为NULL，此时继续推进下一层的调用。

```c++
class Solution {
public:
	int sumVal(TreeNode* root, int sum) {//sum中存的是root节点之上的所有路径节点总和
		if (!root)
		{
			return 0;
		}
		sum = sum * 10 + root->val;
		if (!root->left && !root->right)//到达叶子节点
		{
			return sum;
		}
		return sumVal(root->left, sum) + sumVal(root->right, sum);
	}
	int sumNumbers(TreeNode* root) {
		if (!root)
		{
			return 0;
		}
		return sumVal(root, 0);
	}

};
```

