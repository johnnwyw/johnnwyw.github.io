---
layout:     post
title:      "Maximum Depth of Binary Tree"
subtitle:   "二叉树深度"
date:       2016-04-02 12:00:00
author:     "Johnnwen"
header-img: "img/post-bg-binaryTree.jpg"
catalog:    true
tags:
    - 非递归
    - leetcode
    - 二叉树
    - c++
    
---

## 104. Maximum Depth of Binary Tree

#### 题意

Given a binary tree, find its maximum depth.<br>

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.


#### 递归解法

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == NULL) return 0;  
        return max(maxDepth(root->left), maxDepth(root->right))+1;  
    }
};
```

#### 非递归解法


```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == NULL)  
            return 0;  
        else root->val = 1;  
        queue<TreeNode*> q;  
        q.push(root);  
        while(!q.empty())  
        {  
            TreeNode* curNode = q.front();  
            q.pop();  
            if(q.empty() && !curNode->left && !curNode->right)  
                return curNode->val;  
            if(curNode->left)  
            {  
                curNode->left->val = curNode->val+1;  
                q.push(curNode->left);  
            }  
            if(curNode->right)  
            {  
                curNode->right->val = curNode->val+1;  
                q.push(curNode->right);  
            }  
        } 
    }
};
```
