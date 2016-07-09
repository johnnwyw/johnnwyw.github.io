---
layout:     post
title:      "Binary Tree Level Traversal "
subtitle:   "  "
date:       2016-07-08 09:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - c++
    - 数据结构
    - 二叉树
  
    
---

## Binary Tree Level Traversal(二叉树遍历问题)

### 102. Binary Tree Level Order Traversal

#### 题意
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

**For example:**

Given binary tree [3,9,20,null,null,15,7],

```
    3
   / \
  9  20
    /  \
   15   7
```
return its level order traversal as:

```

[
  [3],
  [9,20],
  [15,7]
]
```
#### 代码

```
class Solution {
public:

	// 递归

    /*void levelTra(vector<vector<int>> &res,TreeNode* root,int level){
        if (root == NULL)
            return ;
        if (level == res.size())
        {
            vector<int> temp;
            res.push_back(temp);
        }
        
        res[level].push_back(root->val);
        
        levelTra(res,root->left, level + 1);
        levelTra(res,root->right, level + 1);
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        
        vector<vector<int>> res;
        
        levelTra(res,root,0);
        return res;
        
    }*/
    
    //非递归
    vector< vector<int> > levelOrder(TreeNode *root)
    {
        vector< vector<int>> res;
        if (root == NULL)
            return res;
        queue<TreeNode *> que;
        que.push(root);
        while (!que.empty())
        {
            int size = que.size();
            vector<int> temp_res;
            for (int i = 0; i < size; i++)
            {
                TreeNode *temp = que.front();
                que.pop();
                temp_res.push_back(temp->val);
                if (temp->left != NULL)
                    que.push(temp->left);
                if (temp->right != NULL)
                    que.push(temp->right);
            }
            res.push_back(temp_res);
        }
        return res;
    }
};
```
### 103. Binary Tree Zigzag Level Order Traversal

#### 题意

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

**For example:**

Given binary tree [3,9,20,null,null,15,7],

```
    3
   / \
  9  20
    /  \
   15   7

```

return its zigzag level order traversal a:

```
[
  [3],
  [20,9],
  [15,7]
]
```

####代码

```
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector< vector<int>> res;
        if (root == NULL)
            return res;
        queue<TreeNode *> que;
        que.push(root);
        while (!que.empty())
        {
            int size = que.size();
            vector<int> temp_res;
            for (int i = 0; i < size; i++)
            {
                TreeNode *temp = que.front();
                que.pop();
                temp_res.push_back(temp->val);
                if (temp->left != NULL)
                    que.push(temp->left);
                if (temp->right != NULL)
                    que.push(temp->right);
            }
            res.push_back(temp_res);
        }
         for (int i = 1; i < res.size(); i += 2) {  
            reverse(res[i].begin(), res[i].end());  
        }  
        return res;
        
    }
};
```

#### 107. Binary Tree Level Order Traversal II

####题意

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

***For example:***

Given binary tree [3,9,20,null,null,15,7],


```
    3
   / \
  9  20
    /  \
   15   7
```

return its bottom-up level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
]

```

#### 代码

```

class Solution {
public:

	//非递归
    /*vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector< vector<int>> res;
        if (root == NULL)
            return res;
        queue<TreeNode *> que;
        que.push(root);
        while (!que.empty())
        {
            int size = que.size();
            vector<int> temp_res;
            for (int i = 0; i < size; i++)
            {
                TreeNode *temp = que.front();
                que.pop();
                temp_res.push_back(temp->val);
                if (temp->left != NULL)
                    que.push(temp->left);
                if (temp->right != NULL)
                    que.push(temp->right);
            }
            res.push_back(temp_res);
        }
      
        reverse(res.begin(), res.end());
        return res;
        
    }*/
    
    //递归
    
    void levelTra(vector<vector<int>> &res,TreeNode* root,int level){
        if (root == NULL)
            return ;
        if (level == res.size())
        {
            vector<int> temp;
            res.push_back(temp);
        }
        
        res[level].push_back(root->val);
        
        levelTra(res,root->left, level + 1);
        levelTra(res,root->right, level + 1);
    }
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        
        vector<vector<int>> res;
        
        levelTra(res,root,0);
        reverse(res.begin(), res.end());
        return res;
        
    }
};

```
### leetcode 314 Binary Tree Vertical Order Traversal 

[参考资料](https://leetcode.com/discuss/85987/14-lines-concise-and-easy-understand-c-solution
)

#### 题意
Given a binary tree, return the vertical order traversal of its nodes' values. (ie, from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from left to right.

**Examples:**

Given binary tree [3,9,20,null,null,15,7],

```
  3
   / \
  9  20
    /  \
   15   7
```
return its vertical order traversal as:


```
[
  [9],
  [3,15],
  [20],
  [7]
]

```

Given binary tree [3,9,20,4,5,2,7],

```
    _3_
   /   \
  9    20
 / \   / \
4   5 2   7

```

return its vertical order traversal as:

```

[
  [4],
  [9],
  [3,5,2],
  [20],
  [7]
]

```

####分析

* 根节点给个序号0，开始层序遍历
* 左子节点则序号减1
* 右子节点序号加1

* 相同列的节点值放到一起，用一个map来建立序号和其对应的节点值的映射，其可以自动排序

* 用到queue存序号和节点组成的pair

#### 代码

```

class Solution {
public:
    vector<vector<int>> verticalOrder(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) return res;
        map<int, vector<int>> m;
        queue<pair<int, TreeNode*>> q;
        q.push({0, root});
        while (!q.empty()) {
            auto a = q.front(); q.pop();
            m[a.first].push_back(a.second->val);
            if (a.second->left) q.push({a.first - 1, a.second->left});
            if (a.second->right) q.push({a.first + 1, a.second->right});
        }
        for (auto a : m) {
            res.push_back(a.second);
        }
        return res;
    }
};

```



