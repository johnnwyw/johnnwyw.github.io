---
layout:     post
title:      "Invert Binary Tree"
subtitle:   "二叉树反转"
date:       2016-04-02 14:00:00
author:     "Johnnwen"
header-img: "img/post-bg-binaryTree.jpg"
catalog:    true
tags:
    - 非递归
    - leetcode
    - 二叉树
    - c++
    
---


## 226. Invert Binary Tree


#### 题意


Invert a binary tree.


	     4
	   /   \
	  2     7
	 / \   / \
	1   3 6   9
	
	
**to**


	     4
	   /   \
	  7     2
	 / \   / \
	9   6 3   1
	
	
#### 递归解法

```
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == NULL){
            return NULL;
        }
     
        TreeNode* pTempNode = root->left;
        root->left = invertTree(root->right);
        root->right = invertTree(pTempNode);
        return root;

        
    }
};
```

#### 非递归解法

```
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        queue<TreeNode*> queue;
        if (root == NULL)  
            return NULL;  
        queue.push(root);  
        while(!queue.empty()){  
            TreeNode * pNode = queue.front();  
            queue.pop();  
            TreeNode * pLeft = pNode->left;  
            pNode->left = pNode->right;  
            pNode->right = pLeft;  
            if (pNode->left)  
                queue.push(pNode->left);  
            if (pNode->right)  
                queue.push(pNode->right);  
        }  
        return root;
        
    }
};
```