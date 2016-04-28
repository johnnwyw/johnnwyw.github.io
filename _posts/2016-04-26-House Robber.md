---
layout:     post
title:      "House Robber "
subtitle:   "  "
date:       2016-04-26 08:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - DFS
    - DP
    - 数据结构
    - c++
    
---


### House Robber

#### 198. House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

##### 分析
**动态规划(dynamic programming)**

##### 代码

```
class Solution {  
public:  
    int rob(vector<int>& nums) {  
        int n = nums.size();
        if(nums.empty()) return 0;  
        vector<int> money(nums.size(), 0);  
        money[0] = nums[0];
        money[1] = max(money[0], nums[1]);
        for(int i=2; i<n; i++) {  
            money[i] = max(money[i-1], money[i-2]+nums[i]);  
        }  
        return money[n-1];  
    }  
}; 

```
**优化时间复杂度的代码**


```

class Solution {  
public:  
    int rob(vector<int>& nums) { 
        int n = nums.size();
        if(nums.empty()) return 0;  
        int money1 = 0, money2 = 0, money3 = 0;  
        for(int i=0; i<nums.size(); i++) {  
            money3 = max(money2, money1+nums[i]);  
            money1 = money2;  
            money2 = money3;  
        }  
        return money3;  
    }  
};

```
 
#### 213. House Robber II 
 
After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

##### 分析

**动态规划(dynamic programming)**

198题的升级版

* 如果偷了第一个房子就不能偷最后一个房子，这样相当于求第一个到倒数第二个房子能偷到的最大钱数；
* 如果不偷第一个房子就可以偷最后一个房子，相当于求第二个到最后一个房子能偷到的最大钱数。


##### 代码

 
```

class Solution {  
public:  
    int dp(vector<int>& nums,int begin,int end) { 
        int n = end-begin;
        vector<int> money(n, 0); 
        money[0] = nums[begin];
        money[1] = max(money[0], nums[begin+1]);
        int count = begin+2;
        for(int i=2; i<n; i++) {  
            money[i] = max(money[i-1], money[i-2]+nums[count]);  
            count++;
        }  
        return money[n-1];  
    }  
    int rob(vector<int>& nums) { 
        int n = nums.size();
        if(nums.empty()) return 0;  
        if(n ==1)
            return nums[0];
        
        return  max(nums[0]+dp(nums,2,n-1),dp(nums,1,n));  
    }  
}; 

//优化时间复杂度的代码
/*class Solution {  
public:  
    int dp(vector<int>& nums,int begin,int end) { 
        int money1 = 0, money2 = 0, money3 = 0;  
        for(int i=begin; i<end; i++) {  
            money3 = max(money2, money1+nums[i]);  
            money1 = money2;  
            money2 = money3;  
        }  
        return money3;  
    }  
    int rob(vector<int>& nums) { 
        int n = nums.size();
        if(nums.empty()) return 0;  
        if(n ==1)
            return nums[0];
        
        return max(nums[0]+dp(nums,2,n-1),dp(nums,1,n));  
    }  
};*/

```

#### House Robber III 

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

**Example 1:**

```
     3
    / \
   2   3
    \   \ 
     3   1
```

Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

**Example 2:**

```
     3
    / \
   4   5
  / \   \ 
 1   3   1
 
```
 Maximum amount of money the thief can rob = 4 + 5 = 9.

##### 分析

**DFS（Depth-First-Search）**

##### 代码
```
class Solution {
public:

    vector<int> DFS(TreeNode* node) {  
        vector<int> res(2, 0);  
        if(!node) return res;  
        vector<int> lRes = DFS(node->left);  
        vector<int> rRes = DFS(node->right);  
        res[0] = lRes[1] + rRes[1] + node->val;  
        res[1] = max(lRes[0], lRes[1]) + max(rRes[0], rRes[1]);  
        return res;  
    }  
    
    int rob(TreeNode* root) {
        vector<int> res = DFS(root);  
        return max(res[0], res[1]);  
        
    }
};
```