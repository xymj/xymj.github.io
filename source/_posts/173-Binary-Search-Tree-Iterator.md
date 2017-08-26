---
title: 173. Binary Search Tree Iterator
date: 2017-08-26 16:13:24
tags: LeetCode
categories: LeetCode
---

Implement an iterator over a binary search tree(BST).Your iterator will be initialized with the root node of a BST.
Calling next() will return the next smallest number in the BST.

Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

<!--more-->

## 题意：

　　在二叉搜索树（BST）上实现一个迭代器。迭代器的初始化值是二叉搜索树的根节点。
　　调用next()函数将返回二叉搜索树的下一个更小的元素。
　　**注意：**next() and hasNext() 函数运行的时间复杂度平均O（1），空间复杂度O（h），其中h是树的高度。

## 思路：

　　主要借助栈完成儿茶搜索树的中序遍历，并且用全局树节点指针记录栈顶元素，挥着栈顶元素的右子树，通过全局树节点指针和栈是否为空判断迭代器是否可以获取元素，用next()函数进行取值。

```c++
/**
* Definition for binary tree
**/
struct TreeNode {
     int val;
     TreeNode *left;
     TreeNode *right;
     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
class BSTIterator {
public:
	BSTIterator(TreeNode *root) {
       bstRoot = root; 
	}

	/** @return whether we have a next smallest number */
	bool hasNext() {
       return (bstRoot ||!s.empty());
	}

	/** @return the next smallest number */
	int next() {
      int res = 0;
      while(bstRoot)
      {
        s.push(bstRoot);
        bstRoot = bstRoot->left;
	  }
      TreeNode *temp = s.top();
      res = temp->val;
      s.pop();
      bstRoot = temp->right;
      return res;
	}
private:
    stack<TreeNode*> s;
    TreeNode *bstRoot;
};
```

