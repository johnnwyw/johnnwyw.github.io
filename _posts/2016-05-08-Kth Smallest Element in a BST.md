---
layout:     post
title:      "leetcode 230. Kth Smallest Element in a BST  "
subtitle:   "  "
date:       2016-05-08 08:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - 二叉树
    - 数据结构
    - c++
    - BST
    
---


### leetcode 230. Kth Smallest Element in a BST 

#### 题意

Given a binary search tree, write a function **kthSmallest** to find the kth smallest element in it.

***Note:*** 
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Follow up:**
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

**Hint:**

1. Try to utilize the property of a BST.
2. What if you could modify the BST node's structure?
3. The optimal runtime complexity is O(height of BST).


##### 分析

* 二叉查找树中序遍历后的结果是有序递增的

* 根据题意，在中序遍历过程中利用计数count器计数，当count＝k的时候，就是二叉树中第k小的节点。




##### 代码

##### 递归解法

```

class Solution {
    int count = 0;
    int res = 0;
public:
    
    int kthSmallest(TreeNode* root, int k) {
        if(root->left != NULL){
            kthSmallest(root->left,k);
        }
        count++;
        if(count == k){
            res = root->val;
        }
        if(root->right != NULL){
            kthSmallest(root->right,k);
        }
        
   
        
        return res;
    }
};

/*
class Solution {  
public:  
    int calcTreeSize(TreeNode* root){  
        if (root == NULL)  
            return 0;  
        return 1+calcTreeSize(root->left) + calcTreeSize(root->right);          
    }  
    int kthSmallest(TreeNode* root, int k) {  
        if (root == NULL)  
            return 0;  
        int leftSize = calcTreeSize(root->left);  
        if (leftSize == k-1){  
            return root->val;  
        }else if (leftSize >= k){  
            return kthSmallest(root->left,k);  
        }else{  
            return kthSmallest(root->right, k-leftSize-1);  
        }  
    }  
};  */



```

##### 非递归解法

```
class Solution {
    int count = 0;
    int res = 0;
public:
    
    int kthSmallest(TreeNode* root, int k) {
        
        stack<TreeNode*> s;
        TreeNode *p=root;
        while(p!=NULL||!s.empty())
        {
            while(p!=NULL)
            {
                s.push(p);
                p=p->left;
            }
            if(!s.empty())
            {
                p=s.top();
                count++;
                if(count == k){
                    res = p->val;
                }
                s.pop();
                p=p->right;
            }
        }    
        
        return res;
    }
};
```
