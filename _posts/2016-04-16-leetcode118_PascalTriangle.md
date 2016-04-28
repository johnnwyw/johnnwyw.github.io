---
layout:     post
title:      "Pascal's Triangle "
subtitle:   "杨辉三角形 "
date:       2016-04-15 11:00:00
author:     "Johnnwen"
header-img: "img/kobe1.jpg"
catalog:    true
tags:
    - leetcode
    - 数据结构
    - c++
    
---


### 118. Pascal's Triangle 

##### 题意

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,

Return

```

[
     [1],     
    [1,1],    
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

##### 分析

每一层的第i个数等于上一层第i-1个数与第i个数之和

##### 代码

```
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int> > res;
        if(numRows == 0)
            return res;
            
        vector<int> cur(1,1);
        res.push_back(cur);
        vector<int> pre = cur;
        
        for(int i = 2; i <= numRows; i ++)
        {
            pre.push_back(0);
            cur = pre;
            for(int j = 1; j < i; j ++)
            {
                cur[j] = pre[j] + pre[j-1];
            }
            res.push_back(cur);
            pre = cur;
        }
        return res;
        
    }
};
```
