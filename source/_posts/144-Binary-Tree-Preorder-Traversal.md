---
title: 144. Binary Tree Preorder Traversal
date: 2017-08-23 22:56:47
tags: LeetCode
categories: LeetCode
---

Given a binary tree, return the preorder traversal of its nodes' values.

```c++
For example :
Given binary tree{ 1,#,2,3 },

       1
        \
          2
         /
       3
return[1, 2, 3].
```

<!--more-->

## 题意：

　　二叉树的先根序遍历。

## 思路：

### 　方法一：

　　二叉树的先根序遍历递归实现。

```c++
struct TreeNode {
	int val;
	TreeNode *left, *right;
	TreeNode(int x) :val(x), left(NULL), right(NULL) {}
};
//二叉树的先序遍历的递归形式
//递归操作：(递归操作就像栈存储数据一样，先进后出)
//思想：若二叉树为空，返回。否则   1）遍历根节点；2）先序遍历左子树；3）先序遍历右子树
class Solution
{
  vector<int>  resVals;
public:
    void preOrderRecursive(TreeNode *root) {
        if (!root)
        {
            return;
        }
        resVals.push_back(root->val);//遍历数据,放入结果集
        preOrderRecursive(root->left);//递归调用，先序遍历左子树  
        preOrderRecursive(root->right);//递归调用，先序遍历右子树  
    }  
};
```

### 　方法二：

　　二叉树的先根序遍历非递归实现,借助栈。

```c++
class Solution
{
public:
    vector<int> preorderTraversal1(TreeNode* root) {
        vector<int> vals;
        if (!root)
        {
            return vals;
        }
        stack<TreeNode*> s;
      	TreeNode *tmp=root;
        while (tmp || !s.empty())
        {
          	if(tmp)
            {
                vals.push_back(tmp->val);
              	s.push(tmp);
              	tmp = tmp->left;
            }
          	else
            {
                tmp = s.top();
               	s.pop();
              	tmp = tmp->right;
            }
        }
        return vals;
    }
}；
```

