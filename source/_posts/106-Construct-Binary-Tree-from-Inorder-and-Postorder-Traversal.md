---
title: 106. Construct Binary Tree from Inorder and Postorder Traversal
date: 2017-08-14 22:32:02
tags: LeetCode
categories: LeetCode
---

# 106. Construct Binary Tree from Inorder and Postorder Traversal

Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

<!--more-->

## 题意：

　　给定一个中序和后序遍历序列，构建一个二叉树。
　　**注意：**二叉树中元素无重复元素。

## 思路：

　　后序遍历中根的顺序是从后往前的，后序遍历最后一个元素是根结点（k），因此遍历后序数组时，应当从后往前搜索，在中序遍历序列中找值为k的下标idx，idx将中序遍历序列分成左右子树，可进行递归操作，后序遍历时先经过右子树的根再经过左子树的根。因此，递归时先递归遍历右子树，再递归遍历左子树。最后把得到的两个子树接到当前的根结点上。

```c++
          1       
         / \   
        2   3   
       / \ / \   
      4  5 6  7
对于上图的树来说，
         index: 0 1 2 3 4 5 6
     中序遍历为: 4 2 5 1 6 3 7
     后续遍历为: 4 5 2 6 7 3 1
可以发现的规律是：
1. 中序遍历中根节点是左子树右子树的分割点。
2. 后续遍历的最后一个节点为根节点。

同样根据中序遍历找到根节点的位置，然后顺势计算出左子树串的长度。在后序遍历中分割出左子树串和右子树串，递归的建立左子树和右子树。
```

```c++
class Solution {
	unordered_map<int, int> inorderHash;//声明为类的私有变量，类方法都可以使用，不用形参复制传递，避免造成超时现象
public:
	TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
		if (inorder.empty()||postorder.empty())
		{
			return NULL;
		}
		//unordered_map<int, int> inorderHash;//把这个映射表放在函数内，以形参的形式复制传递会造成内存泄漏，造成//Status: Time Limit Exceeded
		for (int i = 0;i<inorder.size();i++)
		{
			inorderHash[inorder[i]] = i;
		}
		return createTree(inorder, 0, inorder.size() - 1, postorder, 0, postorder.size() - 1);
	}
  
	TreeNode* createTree(vector<int>& inorder, int inStartIndex, int inEndIndex, vector<int>& postorder, int postStartIndex, int postEndIndex)
	{
		if (inStartIndex > inEndIndex || postStartIndex > postEndIndex)
		{
			return NULL;
		}
		TreeNode* root = new TreeNode(postorder[postEndIndex]);
		int partition = inorderHash[postorder[postEndIndex]];
		int len = partition - inStartIndex;
		TreeNode* left = createTree(inorder, inStartIndex,partition - 1, postorder, postStartIndex, postStartIndex + len - 1);
		TreeNode* right = createTree(inorder, partition + 1, inEndIndex, postorder, postStartIndex + len, postEndIndex - 1);
		root->left = left;
		root->right = right;
		return root;
	}
};
```

