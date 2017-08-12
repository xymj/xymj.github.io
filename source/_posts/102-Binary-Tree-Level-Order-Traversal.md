---
title: 102. Binary Tree Level Order Traversal
date: 2017-08-12 13:40:35
tags: LeetCode
categories: LeetCode
---

# 102. Binary Tree Level Order Traversal

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree [3,9,20,null,null,15,7],
　　　3
   　　/  　\
　　9     　20
　 /      \
　15     7

return its level order traversal as:
[
　[3],
　[9,20],
　[15,7]
]

<!--more-->

## 题意：

　　层序遍历二叉树。

## 思路：

　　二叉树的层序遍历主要是利用队列来实现，在遍历每一层进行输出结果时可以用两种方式进行控制，一种是利用第二临时队列存储下一层元素，在输出完本层元素后，把这个临时队列再赋值给之前那个队列，有队列赋值操作，效率低。
　　**第二种，**利用队列的大小来控制层的输出，不用用第二个临时队列来存储下层元素，在遍历下层元素的同时可以直接把此元素放入之前的队列中，代码如下：

```c++
class Solution2 {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if(!root)
            return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
			int levelSize = q.size();
            vector<int> level;
            while(levelSize>0){//利用本层元素个数控制本层元素的输出
                TreeNode *temp = q.front();
                q.pop();
                level.push_back(temp->val);
                if(temp->left)
                    q.push(temp->left);
                if(temp->right)
                    q.push(temp->right);

				levelSize--;
            }
            res.push_back(level);
            //q1 = q2;减少队列的赋值操作
        }
        return res;
    }
};
```

