---
title: 116. Populating Next Right Pointers in Each Node
date: 2017-08-17 22:56:07
tags: LeetCode
categories: LeetCode
---

# 116. Populating Next Right Pointers in Each Node

Given a binary tree

​    struct TreeLinkNode {
​      TreeLinkNode *left;
​      TreeLinkNode *right;
​      TreeLinkNode *next;
​    }
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

   You may only use constant extra space.
​    You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

```c++
For example,
Given the following perfect binary tree,
         1
       /  \
      2    3
     / \  / \
    4  5  6  7
After calling your function, the tree should look like:
        1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
```

<!--more-->

## 题意：

　　给定一个二叉树，填充每个节点的next针指向它的下一个右节点。如果没有下一个右节点，则next指针应设置为null。
　　最初，所有next指针都设置为null。

　　**注意：**

　　　　只能使用常量内存空间。
　　　　你可以假设它是一个**完全二叉树**（即所有叶子都在同一层，而且每个父母都有两个孩子）。（完全二叉树降低题目难度）。

## 思路：

　　这道题之所以放上来是因为题目中的那句话：You may only use constant extra space
　　这就意味着，深搜是不能用的，因为递归是需要栈的，因此空间复杂度将是 O(logn)。毫无疑问广搜也不能用，因为队列也是占用空间的，空间占用还高于 O(logn)，难就难在这里，深搜和广搜都不能用。但是核心仍然是广搜，可以借用 next 指针，做到不需要队列就能完成广度搜索。
　　如果当前层所有结点的next 指针已经设置好了，那么据此，下一层所有结点的next指针也可以依次被设置。

```c++
class Solution {
public:
	void connect(TreeLinkNode *root) {
		if (!root)
		{
			return;
		}
		while (root->left)//注意这个地方是root的左子树不能为空，如果左子树为空表明下一层不存在节点，则构造结束
		{
			TreeLinkNode* tempRoot = root;
			while (tempRoot)
			{
				tempRoot->left->next = tempRoot->right;
				if (tempRoot->next)
				{
					tempRoot->right->next = tempRoot->next->left;
				}
				tempRoot = tempRoot->next;
			}
			root = root->left;
		}
	}                             
};
```

# 引申：要连接的二叉树不是一颗完全二叉树题[117. Populating Next Right Pointers in Each Node II](http://blog.taoaili999.cn/2017/08/18/117-Populating-Next-Right-Pointers-in-Each-Node-II/)




