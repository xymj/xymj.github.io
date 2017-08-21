---
title: 113. Path Sum II
date: 2017-08-16 22:10:29
tags: LeetCode
categories: LeetCode
---

# 113. Path Sum II

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

```c++
For example:
Given the below binary tree and sum = 22,
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
return
[
   [5,4,11,2],
   [5,8,4,5]
]
```

<!--more-->

## 题意：

　　给定一个二叉树和一个目标和，找出所有的根到叶的路径，使每个路径的总和等于给定的和。

## 思路：

　　思路同[112. Path Sum](http://blog.taoaili999.cn/2017/08/15/112-Path-Sum/)都是遍历跟到叶节点的路径，不同点是[112. Path Sum](http://blog.taoaili999.cn/2017/08/15/112-Path-Sum/)只要找到一条满足的路径和就行，而此题要求找出所有的路径和等于目标和的路径元素集合。

### 　方法一：　　 

　　递归求解，如果root是叶节点，并且它的值恰好等于sum，直接返回root的值。否则，从root的子树中去找一条到叶节点的路径，且这条路径上的节点的和等于(sum － root->val)。然后我们把root加到这条路径里，就是符合题目要求的一条完整路径。

```c++
class Solution {
public:
	vector<vector<int>> pathSum(TreeNode* root, int sum) {
		vector<vector<int>> res;
		vector<int> path;
		if (!root)
			return res;
		pathSumVec(root, path, res, sum);
		return res;
	}
	void pathSumVec(TreeNode* root, vector<int> path, vector<vector<int>> &pathSum, int sum) {
		if (!root)
			return;
		path.push_back(root->val);
		if (!root->left && !root->right)
		{
			if (sum == root->val)
			{
				pathSum.push_back(path);
			}
		}
		pathSumVec(root->left, path, pathSum, sum - root->val);
		pathSumVec(root->right, path, pathSum, sum - root->val);
	}
};
```

### 　方法二：　　 

　　DFS的递归求和，然后到跟节点后和目标值比较，而不是像上面的方法，是把和减去键值，和叶节点的值进行比较。
　　如果root为空，直接返回空数组，否则调用pathSumVec函数。pathSumVec函数将传入的root节点值压入path数组，更新当前数组path的和值nowSum，如果root为叶子节点，且path数组和为target，则将path数组压入pathSum。对于左子树结点，递归调用pathSumVec(root->left, path, pathSum, sum,nowSum);，对于右子结点，递归调用 pathSumVec(root->right, path, pathSum, sum,nowSum);。请注意，path数组不能传入引用参数，应该传入形参。每个递归函数在运行过程中对path数组的操作都是各自为政，互不干涉的。

```c++
class Solution {
public:
	vector<vector<int>> pathSum(TreeNode* root, int sum) {
		vector<vector<int>> res;
		vector<int> path;
		if (!root)
			return res;
		int nowSum = 0;//一个变量记录路径和，不像方法一中通过sum减去节点值记录
		pathSumVec(root, path, res, sum,nowSum);
		return res;
	}
	void pathSumVec(TreeNode* root, vector<int> path, vector<vector<int>> &pathSum, int sum,int nowSum) {
		if (!root)
			return;
		nowSum += root->val;
		path.push_back(root->val);
		if (!root->left && !root->right)
		{
			
			
			if (sum == nowSum)
			{
				pathSum.push_back(path);
			}
		}
		pathSumVec(root->left, path, pathSum, sum,nowSum);
		pathSumVec(root->right, path, pathSum, sum,nowSum);
	}
};
```

