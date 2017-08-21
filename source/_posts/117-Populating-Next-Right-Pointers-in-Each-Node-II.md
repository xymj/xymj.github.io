---
title: 117. Populating Next Right Pointers in Each Node II
date: 2017-08-18 22:08:49
tags: LeetCode
categories: LeetCode
---

# 117. Populating Next Right Pointers in Each Node II

Follow up for problem "Populating Next Right Pointers in Each Node".
What if the given tree could be any binary tree? Would your previous solution still work?

Note:
　　You may only use constant extra space.

```c++
For example,
Given the following binary tree,
     1
   /  \
  2    3
 / \    \
4   5    7
After calling your function, the tree should look like:
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \    \
4-> 5 -> 7 -> NULL
```

<!--more-->

## 题意：

　　题意同[116. Populating Next Right Pointers in Each Node](http://blog.taoaili999.cn/2017/08/17/116-Populating-Next-Right-Pointers-in-Each-Node/)，题目做了一些更改：就是树不一定是满二叉树。

## 思路：

　　思路同[116. Populating Next Right Pointers in Each Node](http://blog.taoaili999.cn/2017/08/17/116-Populating-Next-Right-Pointers-in-Each-Node/)，**解法的核心：**递推思想依然不需要改变，依然是依据当前层的next 指针，设置下一层的 next 指针。只是找结点麻烦些，我们定义了两个函数，findNextLevelNextNode(root, curNode)用来找(n+1)层的下一个节点，findNextLevelStartNode(root)用来找下一层的起始节点。

```c++
class Solution {
public:
	void connect(TreeLinkNode *root) {
		if (!root)
		{
			return;
		}
		while (root)
		{
			TreeLinkNode* startNode = findNextLevelStartNode(root);//寻找下一层的第一个节点
			TreeLinkNode* curNode = startNode;
			TreeLinkNode* nextNode = findNextLevelNextNode(root, curNode);//寻找下一层的next节点
			while (nextNode)
			{
				curNode->next = nextNode;
				curNode = nextNode;
				nextNode = findNextLevelNextNode(root, curNode);
			}
			root = startNode;
		}
	}
	TreeLinkNode* findNextLevelStartNode(TreeLinkNode* curRoot)
	{
		if (!curRoot)
		{
			return NULL;
		}
		if (curRoot->left)
		{
			return curRoot->left;
		}
		return findNextLevelNextNode(curRoot, curRoot->left);
	}
	TreeLinkNode* findNextLevelNextNode(TreeLinkNode* curRoot,TreeLinkNode* curNode)
	{
		if (curRoot->left==curNode&&curRoot->right)
		{
			return curRoot->right;
		}
		else {
			while (curRoot->next)
			{
				curRoot = curRoot->next;
				if (curRoot->left&&curRoot->left!=curNode)
				{
					return curRoot->left;
				}
				if (curRoot->right&&curRoot->right != curNode)
				{
					return curRoot->right;
				}
			}
		}
		return NULL;
	}
};
```



