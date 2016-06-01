---
layout:     post
title:      "leetcode 46. Permutations   "
subtitle:   "  "
date:       2016-05-31 08:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - 数据结构
    - c++
    - DFS
  
    
---


### leetcode 46. Permutations

#### 题意

Given a collection of distinct numbers, return all possible permutations.

**For example**

[1,2,3] have the following permutations:

[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], and [3,2,1].




##### 代码

###### 解法一（STL<algrithm>）

```
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int> > res;
        if (nums.empty())
            return res;

        sort(nums.begin(), nums.end());
        res.push_back(nums);
        while (next_permutation(nums.begin(), nums.end()))
            res.push_back(nums);

        return res;
        
    }
};

```

###### 解法二（DFS）

```
class Solution {  
private:  
    void dfs(int i, vector<int>& nums,vector<vector<int>>& res) {  
        if(i==nums.size()) { 
            res.push_back(nums);  
            return;  
        }  
        for(int j=i;j<nums.size();j++) {  
            int tmp = nums[i];  
            nums[i]  = nums[j];  
            nums[j]  = tmp;  
            dfs(i+1,nums,res);  
            tmp = nums[i];  
            nums[i]  = nums[j];  
            nums[j]  = tmp;  
        }  
    }  
public:  
    vector<vector<int>> permute(vector<int>& nums) { 
        vector<vector<int> > res;
        
         if (nums.empty())
            return res;
            
        
        dfs(0,nums,res);  
          
        return res;  
    }  
};  
```


