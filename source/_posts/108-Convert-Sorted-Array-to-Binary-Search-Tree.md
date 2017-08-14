---
title: 108. Convert Sorted Array to Binary Search Tree
date: 2017-08-14 23:26:08
tags: LeetCode
categories: LeetCode
---

# 108. Convert Sorted Array to Binary Search Tree

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

<!--more-->

## 题意：

　　给定一个数组，其中的元素按升序排序，将它转换成一个高度平衡的二分搜索树。

## 思路：

 　　给定一个区间[left, right]，取其中值mid=(left+right)/2对应的元素作为二叉树的根，左子树为[left, mid-1]，右子树为[mid+1,right]，递归的创建左右子树。

```c++
class Solution {
public:
	TreeNode* arrayToBST(vector<int>& nums, int left, int right) {
		if (left>right)
		{
			return NULL;
		}
		if (left == right)
		{
			return new TreeNode(nums[left]);
		}
		else {
			int mid = (left + right) / 2;
			TreeNode *root = new TreeNode(nums[mid]);
			root->left = arrayToBST(nums, left, mid - 1);
			root->right = arrayToBST(nums, mid + 1, right);
			return root;
		}
	}
	TreeNode* sortedArrayToBST(vector<int>& nums) {
		return arrayToBST(nums, 0, nums.size() - 1);
	}
};
```

