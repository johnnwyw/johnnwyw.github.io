---
layout:     post
title:      "Binary Tree Traversal"
subtitle:   "二叉树遍历"
date:       2016-04-02 10:00:00
author:     "Johnnwen"
header-img: "img/post-bg-binaryTree.jpg"
catalog:    true
tags:
    - 非递归
    - leetcode
    - 二叉树
    - c++
    
---


## 144.Binary Tree Preorder Traversal

#### 题意

Given a binary tree, return the preorder traversal of its nodes' values.

#### For example:

Given binary tree {1,#,2,3},

**return [1,2,3].**

#### 代码

```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode *> stack;
        if (root != NULL) {
            stack.push(root);
        }
        while (!stack.empty()) {
            TreeNode * tmpNode = stack.top();
            stack.pop();
            result.push_back(tmpNode->val);
            
            if (tmpNode->right) {
                stack.push(tmpNode->right);
            }

            if (tmpNode->left) {
                stack.push(tmpNode->left);
            }
        }
        return result;
        
        
    }
};
```

#### 分析

如果用递归算法实现，只需如下代码:

```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
    	vector<int> result;
    	if(!root){
    		return result;
    	}    	
    	result.push_back(tmpNode->val);
    	if(root->left)
    		preorderTraversal(root->left);
    	}
    	if(root->right){
    		preorderTraversal(root->right);
    	}
    	return result;    
    }
};

```

递归的解法十分简单，然后题目强调不能使用递归实现，非递归实现前序遍历大体思路如下：<br>

	1、如果根节点非空，将根节点加入到栈中
	2、如果栈不空，弹出出栈顶节点，将其值加加入vector容器中。
		如果该节点的右子树不为空，将右子节点加入栈中。
		如果左子节点不为空，将左子节点加入栈中。
	3、重复第二步，直到栈空。
	
## 94. Binary Tree Inorder Traversal

#### 题意

Given a binary tree, return the preorder traversal of its nodes' values.

#### For example:

Given binary tree {1,#,2,3},

**return [1,3,2].**

#### 代码

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode *> stack;  
        TreeNode *pnode = root;  
        vector<int> vector; 
        while(pnode || !stack.empty()) {  
            while(pnode) {  
                stack.push(pnode);  
                pnode = pnode->left;  
            }  
            if(!stack.empty()) {  
                pnode = stack.top(); 
                stack.pop(); 
                vector.push_back(pnode->val);  
                pnode = pnode->right;  
            }  
        }  
        return vector;  
    }
};
```

#### 分析

如果用递归算法实现，只需如下代码:

```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
    	vector<int> result;
    	if(!root){
    		return result;
    	}    	
    	if(root->left)
    		preorderTraversal(root->left);
    	}
    	result.push_back(tmpNode->val);
    	if(root->right){
    		preorderTraversal(root->right);
    	}
    	return result;    
    }
};

```

递归的解法十分简单，然后同上题一样题目强调不能使用递归实现，非递归实现前序遍历大体思路如下：<br>

	1、如果根节点空，则直接返回空的vector
	2、左子树压栈操作（如果左子树非空，一直执行压栈操作，直到左子树为空）
	3、如果栈不空，弹出栈顶节点，将其值加入vector容器中，并且遍历当前节点的右子树。
	4、重复第2、3步。


	
## 145. Binary Tree Postorder Traversal

#### 题意

Given a binary tree, return the preorder traversal of its nodes' values.

#### For example:

Given binary tree {1,#,2,3},

**return [3,2,1].**

#### 代码
	
```	
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> restor;
        stack<TreeNode *> stack;
        TreeNode *temp = root, *lastVisited = NULL;
        while(temp || !stack.empty()){
            while (temp) {
                stack.push(temp);
                temp = temp->left;
            }
            if(!stack.empty()){
                temp = stack.top(); 
                if(temp->right != NULL && temp->right != lastVisited)
                    temp = temp->right;
                else{
                    stack.pop();
                    restor.push_back(temp->val);
                    lastVisited = temp;
                    temp = NULL;
                }
            }

            
        }
        return restor;
        
    }
};
```

#### 分析

	1、如果根节点空，则直接返回空的vector容器
	2、左子树压栈操作（如果左子树非空，一直执行压栈操作，直到左子树为空）
	3、如果栈不空，栈顶节点的右子树不空且右子树还没有访问过，则遍历该节点的右子树；否则从栈顶弹出节点，将其值加入vector容器中，设置该节点已被访问标志。
	4、重复第2、3步。
	







	
	
	


