---
title: 105. Construct Binary Tree from Preorder and Inorder Traversal
date: 2017-08-14 22:30:18
tags: LeetCode
categories: LeetCode
---

# 105. Construct Binary Tree from Preorder and Inorder Traversal

Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

<!--more-->

## 题意：

　　给定一个先序和中序遍历序列，构建一个二叉树。
　　**注意：**二叉树中元素无重复元素。

## 思路：

　　参照[106. Construct Binary Tree from Inorder and Postorder Traversal](http://blog.taoaili999.cn/2017/08/14/106-Construct-Binary-Tree-from-Inorder-and-Postorder-Traversal/)构造树的思想，先序遍历中根的顺序是从前往后的，先序遍历第一个元素是根结点（k），在中序遍历序列中找值为k的下标idx，idx将中序遍历序列分成左右子树，可进行递归操作。

```c++
      1       
     / \   
    2   3   
   / \ / \   
  4  5 6  7
对于上图的树来说
     index: 0 1 2 3 4 5 6
 中序遍历为: 4 2 5 1 6 3 7
 先续遍历为: 1 2 4 5 3 6 7
   
可以发现的规律是：
1. 中序遍历中根节点是左子树右子树的分割点。
2. 先续遍历的第一个节点为根节点。

同样根据中序遍历找到根节点的位置，然后顺势计算出左子树串的长度。在先序遍历中分割出左子树串和右子树串，递归的建立左子树和右子树。
```
```c++
class Solution {
	unordered_map<int, int> hash;
public:
	TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
		if (preorder.empty() || inorder.empty())
			return NULL;
		for (int i = 0; i<inorder.size(); i++)
			hash[inorder[i]] = i;
		return createTree(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1);
	}
	TreeNode* createTree(vector<int>& preorder, int preBeg, int preEnd, vector<int>& inorder, int inBeg, int inEnd) {
		if (preBeg>preEnd || inBeg>inEnd)
			return NULL;
		TreeNode *root = new TreeNode(preorder[preBeg]);
		int partition = hash[preorder[preBeg]];
		int len = inEnd - partition;
		TreeNode *right = createTree(preorder, preEnd - len + 1, preEnd, inorder, partition + 1, inEnd);
		TreeNode *left = createTree(preorder, preBeg + 1, preEnd - len, inorder, inBeg, partition - 1);
		root->left = left;
		root->right = right;
		return root;
	}
};
```

