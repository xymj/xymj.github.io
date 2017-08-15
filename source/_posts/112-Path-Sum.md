---
title: 112. Path Sum
date: 2017-08-15 23:10:02
tags: LeetCode
categories: LeetCode
---

# 112. Path Sum

 Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

```c++
For example:
Given the below binary tree and sum = 22,
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
```

<!--more-->

## 题意：

　　给定一个二叉树和一个和，确定树是否有根节点到叶节点的路径，这样路径上元素累加和等于给定的和。

## 思路：

### 　方法一：

　　 递归的遍历二叉树，当遇到叶子节点(!root->left && !root->right)时，说明找到一条路径，存储路径节点值，把所有路径都找出来，然后相加比较，时间复杂度高，但是此处的递归查找路径上的元素的方法值得学习。

```c++
class Solution {
public:
	bool hasPathSum(TreeNode* root, int sum) {
		if (!root)
			return false;
		vector<vector<int>> path;
		vector<int> pathNum;
		pathNum.push_back(root->val);
		getPath(path, pathNum, root);
		for (auto s : path)//遍历所有路径，求路径元素和
		{
			//cout << s.size()<<endl;
			int sumVal = 0;
			for (int val : s)
			{
				sumVal += val;
			}
			if (sumVal==sum)
			{
				return true;
			}
		}
		return false;
	}
	void getPath(vector<vector<int>> &path, vector<int> pathNum, TreeNode* root) {
		if (!root->left && !root->right)//到达叶子节点
		{
			// pathNum.push_back(root->val);
			path.push_back(pathNum);//记录加入路径元素
			return;
		}
		if (root->left)//此节点左子树存在，则递归进入左子树，直到遇见根节点
		{
			pathNum.push_back(root->left->val);
			getPath(path, pathNum, root->left);
			pathNum.pop_back();//当一直到叶子节点返回，要把上一个路径叶节点元素弹出路径集合
		}
		if (root->right)//此节点右子树存在，则递归进入右子树，直到遇见根节点
		{
			pathNum.push_back(root->right->val);
			getPath(path, pathNum, root->right);
			pathNum.pop_back();//当一直到叶子节点返回，要把上一个路径叶节点元素弹出路径集合
		}
	}
};
```

### 　方法二：

　　 递归过程中直接判断，遍历从根到叶子的每一条路径，并求和。只要相等，就返回true，否则一直遍历下去，最后返回false。与方法一的区别就是不用再记录路径元素，而是直接遍历的过程中求和比较。

```c++
class Solution {
public:
	bool hasPathSum(TreeNode* root, int sum) {
		if (!root)
			return false;
		return getPathSum(root->val,root,sum);
	}
	bool getPathSum(int pathSum,TreeNode* root,int sum) {
		if (!root->left && !root->right)//到达叶子节点
		{
			if (pathSum==sum)
			{
				return true;
			}
		}
		bool lp = false, rp = false;
		if (root->left)//左子树为着，递归遍历左子树，遍历过程中求和
		{
			lp = getPathSum(pathSum + root->left->val, root->left, sum);
		}
		if (root->right)
		{
			rp = getPathSum(pathSum + root->right->val, root->right, sum);
		}
		return (lp || rp);//左右子树有一个和满足就可以
	}
};
```

### 　方法三：

　　 思路同方法二，但是不是通过累加树节点到叶节点求和，而是每层递归的时候，把和减去当前层节点值，然后进入下一层，如果sum减到叶节点的时候，等于叶节点的值，则存在目标和为sum的路径，否则没有，这种目标值反向相减再比较的思路值得学习。

```c++
class Solution4 {
public:
	bool hasPathSum(TreeNode* root, int sum) {
		if (!root)
			return false;
		if (!root->left&&!root->right)
		{
			if (root->val == sum)
			{
				return true;
			}
		}
		return hasPathSum(root->left, sum-root->val)||hasPathSum(root->right,sum-root->val);
	}
	
};
```

