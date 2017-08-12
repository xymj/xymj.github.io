---
title: 99. Recover Binary Search Tree
date: 2017-08-12 12:02:42
tags: LeetCode
categories: LeetCode
---

# 99. Recover Binary Search Tree

Two elements of a binary search tree(BST) are swapped by mistake.
Recover the tree without changing its structure.

Note:
A solution using O(n) space is pretty straight forward.Could you devise a constant space solution ?


<!--more-->

## 题意：

　　二叉搜索树（BST）的两个元素被错误地交换，希望在不改树变结构的情况下恢复二叉搜索树。

　　**注意：**使用O（n）空间的解相当直接，你能设计出一个常数空间解吗？

## 思路：

### 　方法一：

　　非递归的中序遍历：
　　　中序遍历二叉树，出现的节点的值会升序排序，如果有两个节点位置错了，那肯定会出现降序。
　　　设置一个pre节点指针，记录当前节点中序遍历时的前节点，如果当前节点小于pre节点的值，说明需要调整次序。
　　　如果在中序遍历时节点值出现了两次降序，第一个错误的节点为第一次降序时较大的节点，第二个错误节点为第二次降序时较小的节点。
　　　比如，原来的搜索二叉树在中序遍历的节点值依次为{1,2,3,4,5}，如果因为两个节点位置错了而出现{1,5,3,4,2}，
　　　第一次降序为5->3，所以第一个错误节点为5，第二次降序为4->2,所以第二个错误节点为2，将5和2换过来就可以恢复。

　　切记要求空间复杂度为常数，不是O(n)

```c++
class Solution {	
public:
	void recoverTree(TreeNode* root) {
		TreeNode* misNode1 = NULL;
		TreeNode* misNode2 = NULL;
		TreeNode* pre = NULL;
		stack<TreeNode*> s;
		TreeNode* temp = root;
		while (temp || !s.empty())
		{
			if (temp)
			{
				s.push(temp);
				temp = temp->left;
			}
			else {
				temp = s.top();
				s.pop();
				//cout << temp->val << endl;
				if (pre)
				{
					if (pre->val > temp->val)
					{
						if (!misNode1)//此处misNode1为空，说明找到第一个错误节点
						{
							misNode1 = pre;
							misNode2 = temp;//在 找到第一个错误节点时，把其相邻节点设置为错误节点2，是为了控制处理两个错误节点相邻的情况
						}
                        else
						    misNode2 = temp;
					}
				}
				pre = temp;
				temp = temp->right;
			}
		}
		swap(misNode1->val, misNode2->val);
	}
};
```

### 　方法二：

　　递归的中序遍历：
　　　如果把这棵二叉搜索树各结点的值用中序遍历的方式打印出来，那么就有一个位置上的数大于它后面相邻的数，之后又有一个位置上的数小于它前面相邻的数。这两个数正好相邻的情况再考虑。
　　　只需先找出那个大于它后一个数的数misNode1，再找出那个小于它前一个数的数misNode2，之后再交换两者的值就行了。要实现这一点，我们需要保存前一个结点pre，然后找到pre->val > cur->val这种反常的情况（共两次）。此时，若misNode1已经被找到了，让misNode1= pre，此时也设置misNode2 = cur。
　　　接下来考虑两个数相邻的情况。由于两个数相邻，我们只能找到一次 pre->val > cur->val 的情况，无法对misNode2进行赋值。但我们注意到，此时cur刚好就是我们要找的misNode2。再回到一般情况，就算此时的cur不是我们要找的misNode2，我们在后面第二次出现pre->val > cur->val的情况时，我们依旧会对misNode2赋值。所以，我们在第一次遇到pre->val > cur->val的情况时，分别将pre和cur赋值给misNode1和misNode2，在后面如果再次遇到pre->val > cur->val的情况，我们再将misNode2修改为正确的值，并保持misNode1的值不变。这样，我们总能找到正确的misNode2和misNode2。　

```c++
class Solution2 {
	TreeNode* misNode1;
	TreeNode* misNode2;
	TreeNode* pre;
public:
	void recoverTree(TreeNode* root) {
		misNode1 = misNode2 = pre = NULL;//pre必须设为null，通过遍历的时候设进去。因为是中序遍历，所以pre应该是深层叶子左子树的父节点。 
		inOrderFindNode(root);
		swap(misNode1->val, misNode2->val);
	}
	void inOrderFindNode(TreeNode* root)//递归中序遍历
	{
		if (!root)
		{
			return;
		}
		inOrderFindNode(root->left);
		if (pre)//当前驱结点不为空的时候再进行比较
		{
			if (pre->val>root->val)
			{
				if (!misNode1)
				{
					misNode1 = pre;
					misNode2 = root;
				}
				else
					misNode2 = root;
			}
		}
		pre = root;
		inOrderFindNode(root->right);
	}
};
```

