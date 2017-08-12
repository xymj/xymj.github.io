---
title: 101. Symmetric Tree
date: 2017-08-12 13:06:41
tags: LeetCode
categories: LeetCode
---

# 101. Symmetric Tree

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:
![101-1-Symmetric-Tree](/images/101-1-Symmetric-Tree.png)
But the following [1,2,2,null,3,null,3] is not:
![101-2-Symmetric-Tree](/images/101-2-Symmetric-Tree.png)
Note:
Bonus points if you could solve it both recursively and iteratively. 

<!--more-->

## 题意：

　　 给定一个二叉树，检查它是否是自身的镜像（即围绕其中心轴对称）。 

## 思路：

### 　方法一：

　　递归实现，思路比较简练，只要注意控制好递归结束的条件：
　　　　1.停止条件是 left\==NULL  &&  right==NULL    return  true;
　　　　2.停止条件是 left\!=NULL  ||  right != NULL   return false;
　　　　3.left->val == right->val 比较left->left和right.right   &&   left->right和right->left;
　　　　4.任何条件不成立都是false;

```c++
class Solution {
public:
	bool isSymmetric(TreeNode* root) {
		if (!root)
		{
			return true;
		}
		return isLeftAndRightSymmetric(root->left, root->right);
	}
	bool isLeftAndRightSymmetric(TreeNode* left, TreeNode* right)
	{
		if (!left && !right)
		{
			return true;
		}
		else if (!left || !right)
			return false;
		else if (left->val != right->val)
			return false;
		return isLeftAndRightSymmetric(left->left, right->right) && isLeftAndRightSymmetric(left->right, right->left);
	}
};
```

### 　方法二：

　　非递归两个队列实现，层序遍历的左右队列中左右子树进行相互比较，两个队列，分别存储每一层根节点的下一层的左右子树，右左子树：

```c++
class Solution {
public:
	bool isSymmetric(TreeNode* root) {
		if (!root)
		{
			return true;
		}
		queue<TreeNode*> q1, q2;
		q1.push(root->left);
		q2.push(root->right);
		while (!q1.empty()&&!q2.empty())
		{
			TreeNode* q1Node = q1.front();
			q1.pop();
			TreeNode* q2Node = q2.front();
			q2.pop();
			if (!q1Node && !q2Node)
				continue;
			else if (!q1Node || !q2Node)
			{
				return false;
			}
			else if (q1Node->val!=q2Node->val)
			{
				return false;
			}
			else
			{
				q1.push(q1Node->left);//注意两个队列放入节点顺序q1是先left再right
				q1.push(q1Node->right);
				q2.push(q2Node->right);//注意两个队列放入节点顺序q2是先right再left
				q2.push(q2Node->left);
			}
		}
		return true;
	}
};
```

### 　方法三：

　　非递栈实现，用一个栈来实现，要注意进栈时的左右子树节点进栈的顺序。

```c++
class Solution {
public:
	bool isSymmetric(TreeNode* root) {
		if (!root)
		{
			return true;
		}
		stack<TreeNode*> s;
		s.push(root->left);
		s.push(root->right);
		while (!s.empty())
		{
			TreeNode* sANode = s.top();
			s.pop();
			TreeNode* sBNode = s.top();
			s.pop();
			if (!sANode && !sBNode)
				continue;
			else if (!sANode || !sBNode)
			{
				return false;
			}
			else if (sANode->val != sBNode->val)
			{
				return false;
			}
			else
			{
				s.push(sANode->left);//先放sANode->left
				s.push(sBNode->right);//再放sBNode->right
				s.push(sANode->right);//先放sANode->right
				s.push(sBNode->left);//再放sBNode->left
			}
		}
		return true;
	}
};
```

