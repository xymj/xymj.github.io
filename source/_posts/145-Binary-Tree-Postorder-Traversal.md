---
title: 145. Binary Tree Postorder Traversal
date: 2017-08-23 23:12:01
tags: LeetCode
categories: LeetCode
---

Given a binary tree, return the postorder traversal of its nodes' values.

```c++
For example :
Given binary tree{ 1,#,2,3 },

             1
               \
               2
              /
             3
return[3, 2, 1].
```

<!--more-->

## 题意：

　　二叉树的后续遍历。

## 思路：

### 　方法一：

　　递归实现。

```c++
class Solution
{
  vector<int>  resVals;
public:
    void postorderTraversal(TreeNode *root) {
        if (!root)
        {
            return;
        }
        postorderTraversal(root->left);//递归调用，后序遍历左子树  
        postorderTraversal(root->right);//递归调用，后序遍历右子树 
        resVals.push_back(root->val);//遍历数据,放入结果集
    }  
};
```

### 　方法二：

　　非递归实现，后续遍历非递归实现的一个难点，就是要有一个临时节点指针来记录右子树节点是否是被访问过的，如果右子树为空，或者被访问过，则当前节点可以进去结果集。

```c++
class Solution
{
public:
	vector<int> postorderTraversal(TreeNode* root) {
        vector<int> vals;
        if (!root)
            return vals;
        stack<TreeNode*> s;
        TreeNode *temp = root;
        TreeNode *preVisited = NULL;//记录被访问的右子树节点
        while(temp || !s.empty())
        {
            if(temp){
                s.push(temp);
                temp = temp -> left;
            }
            else{
                temp = s.top();
                if(temp->right==NULL || temp->right == preVisited){
                    vals.push_back(temp->val);
                    preVisited = temp;
                    s.pop();
                    temp = NULL;
                }
                else
                  temp = temp ->right;
            }
        }
        return vals;
    }
};
```

