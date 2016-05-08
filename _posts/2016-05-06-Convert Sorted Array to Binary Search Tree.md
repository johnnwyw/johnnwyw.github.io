---
layout:     post
title:      "leetcode 108. Convert Sorted Array to Binary Search Tree  "
subtitle:   "  "
date:       2016-05-06 12:00:00
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


### leetcode 108. Convert Sorted Array to Binary Search Tree 

#### 题意

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

Subscribe to see which companies asked this question


##### 代码

##### 递归解法（中序遍历）

```

class Solution {
public:
    TreeNode* buildTree(vector<int> &nums,int start,int end){
        if(start > end){
            return NULL;
        }
        int mid = start + ((end - start) >> 1);
        
       
        
        TreeNode *left = buildTree(nums,start,mid-1);
        
        TreeNode *node = new TreeNode(nums[mid]);
        node->left = left;
        
        node->right = buildTree(nums,mid+1,end);
        
        return node;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
      
        return buildTree(nums, 0, nums.size()-1);
        
    }
};



```

##### 递归解法( 二分法)

```
class Solution {
public:
    TreeNode* buildTree(vector<int> &nums,int start,int end){
        if(start > end){
            return NULL;
        }
        int mid = start + ((end - start) >> 1);
        
        TreeNode *node = new TreeNode(nums[mid]);
        
        node->left = buildTree(nums,start,mid-1);
        node->right = buildTree(nums,mid+1,end);
        
        return node;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
      
        return buildTree(nums, 0, nums.size()-1);
        
    }
};


```

### 扩展

#### leetcode 109. Convert Sorted List to Binary Search Tree 

##### 题意

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

Subscribe to see which companies asked this question

##### 代码

```

class Solution {
public:
    /*TreeNode* buildTree(vector<int> &nums,int start,int end){
        if(start > end){
            return NULL;
        }
        int mid = start + ((end - start) >> 1);
        
        TreeNode *node = new TreeNode(nums[mid]);
        
        node->left = buildTree(nums,start,mid-1);
        node->right = buildTree(nums,mid+1,end);
        
        return node;
    }
    TreeNode* sortedListToBST(ListNode* head) {
        vector<int> nums;
        
        ListNode* node;
        node = head;
        while(node != NULL){
            nums.push_back(node->val);
            node = node->next;
        }
        
        return buildTree(nums,0,nums.size()-1);
        
    }*/
    
    TreeNode *buildTree(ListNode* & head, int start, int end) {  
        if (start > end) 
            return NULL;  
            
        int mid = start + (end - start) / 2; 
        
        TreeNode* left = buildTree(head, start, mid - 1); 
        
        TreeNode* parent = new TreeNode(head->val);  
        parent->left = left; 
        head = head->next; 
        
        TreeNode* right = buildTree(head, mid + 1, end); 
        
        parent->right = right;  
        
        return parent;  
    }  
    
    TreeNode *sortedListToBST(ListNode *head) {  
        int length = 0;  
        ListNode* node = head;  
        while ( node != NULL) {  
            length++;  
            node = node->next;  
        }  
        return buildTree(head, 0, length - 1);      
    }  
};
```
