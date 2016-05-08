---
layout:     post
title:      "leetcode 99. Recover Binary Search Tree  "
subtitle:   "  "
date:       2016-05-07 08:00:00
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


### leetcode 99. Recover Binary Search Tree 

#### 题意

Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.

***Note:***

A solution using O(n) space is pretty straight forward. Could you devise a constant space solution?

confused what "{1,#,2,3}" means:

OJ's Binary Tree Serialization:

The serialization of a binary tree follows a level order traversal, where '#' signifies a path terminator where no node exists below.

```
   1
  / \
 2   3
    /
   4
    \
     5
     
```

The above binary tree is serialized as "{1,2,3,#,#,4,#,#,5}".


##### 分析

* 二叉查找树中序遍历后的结果是有序递增的

* 根据题意，在遍历过程中会有两处出现前一节点大于当前节点的情况，如果出现这种情况，则交换第一次情况中的前一节点和第二次情况中的当前节点即可。

* 有一种特殊情况即：二叉树只有两个节点，此时只会出现一次上面的情况，即可直接交换当前节点和前一节点即可
* 所以第二种情况可以一直迭代。

##### 代码

##### 递归解法

```

class Solution {
public:
    TreeNode* first;
    TreeNode* second;
    TreeNode* prev;
    int last;
    void recoverTree(TreeNode* root) {
        first = NULL;
        second = NULL;
        prev = NULL;
        inorder(root);
        swap(first->val, second->val);
    }
    void inorder(TreeNode* root)
    {
        
        if(root != NULL){
            
            inorder(root->left);
            
            if(prev == NULL){
                prev = root;
            }else{
                if(root->val < prev->val)
                {
                    if(first == NULL)
                        first = prev;
                    second = root;
                }
                
                prev = root;
            }
            
            inorder(root->right);
        }
       
    }
};

```

##### 非递归解法

```
class Solution {
public:
    TreeNode* first;
    TreeNode* second;
    TreeNode* prev;
    int last;
    void recoverTree(TreeNode* root) {
        first = NULL;
        second = NULL;
        prev = NULL;
        inorder(root);
        swap(first->val, second->val);
    }
    void inorder(TreeNode* root)
    {
        
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
                
                if(prev == NULL){
                    prev = p;
                }else{
                    if(p->val < prev->val){
                        if(first == NULL){
                            first = prev;
                        }
                        second = p;
                    }
                    prev = p;
                }
                s.pop();
                p=p->right;
            }
        }  
    }
};
```
 

