---
layout:     post
title:      "Lowest Common Ancestor of Tree "
subtitle:   "  "
date:       2016-04-29 08:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - tree
    - 数据结构
    - c++
    
---


### Lowest Common Ancestor of Tree 

#### leetcode235. Lowest Common Ancestor of a Binary Search Tree

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the [definition of LCA on Wikipedia:](https://en.wikipedia.org/wiki/Lowest_common_ancestor) “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants **(where we allow a node to be a descendant of itself)**.”

```
        _______6______
       /              \
    ___2__          ___8__
   /      \        /      \
   0      _4       7       9
         /  \
         3   5
         
```

**For example**, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.

##### 分析

* 如果p和q比root小, 则LCA必定在左子树

* 如果p和q比root大, 则LCA必定在右子树

* 如果p和q其中一个比root大，一个比root小, 则root即为LCA.

##### 代码

```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root || !p || !q)
            return NULL;
            
        if(max(p->val,q->val) < root->val){
            return lowestCommonAncestor(root->left,p,q);
        }else if(min(p->val,q->val) > root->val){
            return lowestCommonAncestor(root->right,p,q);
        }else{
            return root;
        }
        
    }
};

```
 
#### 236. Lowest Common Ancestor of a Binary Tree  
 
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia:](https://en.wikipedia.org/wiki/Lowest_common_ancestor) “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants **(where we allow a node to be a descendant of itself)**.”

```
        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
         
```
**For example**, the lowest common ancestor (LCA) of nodes 5 and 1 is 3. Another example is LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

##### 分析

**leetcode235题的升级版**

转换为链表最后相同的节点

* 如果是普通二叉树, 而不是BST. 则应该遍历节点, 记录下从root到p节点和q节点的路径. 

* 比较路径,最后一个相同的节点便是LCA.


##### 代码

```
class Solution {
public:

    bool GetNodePath(TreeNode* root, TreeNode* pNode,list<TreeNode*> &path){
        
        path.push_back(root);
        
        if(root == pNode){
            return true;
        }
        
        bool found = false;
        
        if(!found && root->left){
            found = GetNodePath(root->left,pNode,path);
        }
        if(!found && root->right){
            found = GetNodePath(root->right,pNode,path);
        }
        if(!found){
            path.pop_back();
        }
        return found;
    }
    
    TreeNode* GetLastCommonNode(const list<TreeNode*>& path1,const list<TreeNode*>& path2){
        TreeNode* pLast =NULL;
        
        list<TreeNode*>::const_iterator iterator1 = path1.begin();
        list<TreeNode*>::const_iterator iterator2 = path2.begin();
        
        while(iterator1 != path1.end() && iterator2 != path2.end()){
            if(*iterator1 == *iterator2 ){
                pLast = *iterator1;
            }
            else break;
            iterator1++;
            iterator2++;
        }
        return pLast;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL || p == NULL || q == NULL){
            return NULL;
        }
        
        list<TreeNode*> pathp;
        list<TreeNode*> pathq;
        
        GetNodePath(root,p,pathp);
        GetNodePath(root,q,pathq);
        
        return GetLastCommonNode(pathp,pathq);
    }
};
```

### 扩展 Lowest Common Ancestor of Tree 

##### 代码

 
```
class Solution {
public:

    bool GetNodePath(TreeNode* root, TreeNode* pNode,list<TreeNode*> &path){
        
        path.push_back(root);
        
        if(root == pNode){
            return true;
        }
        
        bool found = false;
        
        vector<TreeNode*>::iterator i = root->m_vChildren.begin();
        while(!found && i!=root->m_vChildren.end()){
        	found = GetNodePath(*i,pNode,path);
        	i++;
        }
        if(!found){
            path.pop_back();
        }
        return found;
    }
    
    TreeNode* GetLastCommonNode(const list<TreeNode*>& path1,const list<TreeNode*>& path2){
        TreeNode* pLast =NULL;
        
        list<TreeNode*>::const_iterator iterator1 = path1.begin();
        list<TreeNode*>::const_iterator iterator2 = path2.begin();
        
        while(iterator1 != path1.end() && iterator2 != path2.end()){
            if(*iterator1 == *iterator2 ){
                pLast = *iterator1;
            }
            else break;
            iterator1++;
            iterator2++;
        }
        return pLast;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL || p == NULL || q == NULL){
            return NULL;
        }
        
        list<TreeNode*> pathp;
        list<TreeNode*> pathq;
        
        GetNodePath(root,p,pathp);
        GetNodePath(root,q,pathq);
        
        return GetLastCommonNode(pathp,pathq);
    }
};
```

