---
layout:     post
title:      "leetcode 116. Populating Next Right Pointers in Each Node   "
subtitle:   "  "
date:       2016-05-16 08:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - 数据结构
    - c++
    - 二叉树
    - BFS
    - 递归
  
    
---


### leetcode 116. Populating Next Right Pointers in Each Node  

#### 题意

Given a binary tree

```
  struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

***Note:***

You may only use constant extra space.

You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

**For example**
Given the following perfect binary tree,

```
        1
       /  \
      2    3
     / \  / \
    4  5  6  7
```

After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
    
```



##### 代码

```

// 二叉树的前序遍历递归
/*class Solution {
public:
    void connect(TreeLinkNode *root) {
        
            if(root != NULL ){
                
                
                if(root->left != NULL){
            
                    root->left->next = root->right;
   
                }
            
                if(root->right != NULL && root->next != NULL){
                    root->right->next = root->next->left;
                }
            
                connect(root->left);
            
                connect(root->right);
            }
        
    }
};*/


// 二叉树的前序遍历非递归
/*class Solution {
public:
    void connect(TreeLinkNode *root) {
        stack<TreeLinkNode*> s;
        TreeLinkNode *p=root;
        while(p!=NULL||!s.empty())
        {
            while(p!=NULL)
            {
                if(p->left != NULL){
            
                    p->left->next = p->right;
   
                }
            
                if(p->right != NULL && p->next != NULL){
                    p->right->next = p->next->left;
                }
                s.push(p);
                p=p->left;
            }
            if(!s.empty())
            {
                p=s.top();
                s.pop();
                p=p->right;
            }
        }
    }

};*/

//二叉树的广度优先搜索(DFS)
/*class Solution {
public:
    void connect(TreeLinkNode *root) {
        
        TreeLinkNode* rootTemp;
        while(root != NULL){
           
            rootTemp = root;
            while(rootTemp != NULL){
                if(rootTemp->left !=NULL){
                    rootTemp -> left -> next = rootTemp -> right;
                }
                if(rootTemp->right !=NULL && rootTemp -> next != NULL)
                    rootTemp -> right -> next = rootTemp -> next -> left;
                rootTemp = rootTemp -> next;
            }
            root = root -> left;
        }
    }
};*/


```

### 扩展 

#### leetcode 117. Populating Next Right Pointers in Each Node II

Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

***Note:***

You may only use constant extra space.

**For example**

Given the following binary tree

```
         1
       /  \
      2    3
     / \    \
    4   5    7

```

After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL

```

##### 代码

```
class Solution {
public:
    void connect(TreeLinkNode *root) {
        TreeLinkNode* prev;
        TreeLinkNode* rootTemp;
        TreeLinkNode* next;
        
        while(root) {
            prev = NULL;
            next = NULL;
            rootTemp = root;
            
            while(rootTemp) {
                if(next == NULL) next = rootTemp->left?rootTemp->left:rootTemp->right;
                if(rootTemp->left) {
                    if(prev) 
                        prev->next = rootTemp->left;
                    prev = rootTemp->left;
                }
                if(rootTemp->right) {
                    if(prev) 
                        prev->next = rootTemp->right;
                    prev = rootTemp->right;
                }
                rootTemp = rootTemp->next;
            }
            root = next;
        }
    }
};

```


