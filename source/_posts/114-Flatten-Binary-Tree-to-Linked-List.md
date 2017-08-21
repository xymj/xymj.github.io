---
title: 114. Flatten Binary Tree to Linked List
date: 2017-08-16 22:34:54
tags: LeetCode
categories: LeetCode
---

# 114. Flatten Binary Tree to Linked List

Given a binary tree, flatten it to a linked list in-place.

```c++
For example,
Given
         1
        / \
       2   5
      / \   \
     3   4   6

The flattened tree should look like:
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```

<!--more-->

## 题意：

　　给定一个二叉树，在常量空间内把它转换为一个链表。

## 思路：

### 　方法一：

　　在遍历的过程中，逐渐把根节点的左子树转换为右子树，并且右子树放到左子树的最右面的叶子节点后面，使根节点的左子树为空，然后继续循环遍历以右子树为下一个根节点，循环上面的思路，直到右子树为空。

```c++
class Solution {
public:
	void flatten(TreeNode* root) {
		if (!root || (!root->left && !root->right))
		{
			return;
		}
		while (root)//循环直到root为空
		{
			if (root->left)
			{
				TreeNode *pre = root->left;
				while (pre->right)//寻找左子树的最右叶子节点
				{
					pre = pre->right;
				}
				pre->right = root->right;//把根节点的右子树放到左子树的最右叶子节点的右子节点上
				root->right = root->left;//把根节点的右子树转换为左子树
				root->left = NULL;//左子树置为空
			}
			root = root->right;
		}
	}
};
```

### 　方法二：

　　因为最后flatten的结果是树的前序遍历的结果，所以考虑一边进行前序遍历，一边进行flatten转化。逻辑很重要，在while循环体中，应该先把curNode->right, curNode->left压栈，再去进行flatten。如果颠倒了顺序，就会在flatten时破坏一些还没有被处理的节点，这些节点被压栈，随后就会发生错误。

```c++
class Solution {
public:
	void flatten(TreeNode* root) {
		if (!root || (!root->left && !root->right))
		{
			return;
		}
		stack<TreeNode*> s;
		TreeNode *temp = root;
		TreeNode *curNode = NULL;
		s.push(temp);
		while (!s.empty())
		{
			curNode = s.top();
			s.pop();
			if (curNode->right)//注意先放进去右子节点
			{
				s.push(curNode->right);
			}
			if (curNode->left)//后方左子节点，这样出站的时候就会先出栈左子树节点做为右子树节点
			{
				s.push(curNode->left);
			}
			temp->right = curNode;//temp代表上层根节点
			temp->left = NULL;
			temp = curNode;
		}
	}
};
```

### 　方法三：

　　最简单的思路，时间复杂度高一些，直接进行先根序遍历，然后把这些节点存储在节点数组中，左后遍历数组中每一个节点，使其左子树节点置为空，右子树节点为数组下一个节点。

```c++
class Solution {
private:
	vector<TreeNode*> res;
public:
	void flatten(TreeNode* root) {
		if (!root||(!root->left&&!root->right))
		{
			return;
		}
		stack<TreeNode*> s;
		TreeNode* temp = root;
		while (temp || !s.empty())
		{
			if (temp)
			{
				res.push_back(temp);
				s.push(temp);
				temp = temp->left;
			}
			else
			{
				temp = s.top()->right;
				s.pop();
			}
		}
		for (int i = 0; i < res.size() - 1; i++)
		{
			res[i]->left = NULL;
			res[i]->right = res[i + 1];
		}
	}
};
```

