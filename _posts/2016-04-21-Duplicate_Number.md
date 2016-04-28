---
layout:     post
title:      "Find the Duplicate Number "
subtitle:   "  "
date:       2016-04-20 12:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - 二分法
    - 数据结构
    - c++
    
---


### 287. Find the Duplicate Number

#### 题意

TGiven an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

***Note:***

You must not modify the array (assume the array is read only).

You must use only constant, O(1) extra space.

Your runtime complexity should be less than O(n2).

There is only one duplicate number in the array, but it could be repeated more than once.


#### 思路

**二分法&抽屉原理**

抽屉原理的大概意思是－－－假设你有10本书，要放进9个抽屉，则至少有一个抽屉里有两本书。

在本题中，1～n的n+1个数字，有1个数字会至少重复两次。

比如｛1，2，3，6，4，4，5｝，一共7个数，范围是1~6，其中位数应该是（6+1）/2 = 3，那么，如果小于等于3的数的个数大于3，则重复的数字一定出现在[1，3]之间，否则出现在[4，6]之间。以该数组为例，中位数为3，小于等于3的数一共有3个，大于3的数有4个，所以重复的数字在[4,6]之间。

##### 代码

```
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int begin = 1;
        int end = (int)nums.size() -1;
        
        while(begin<=end){
            int count = 0;
            int mid = (begin + end)>>1;
            for(int i = 0;i<nums.size();i++){
                if(nums[i] <= mid)
                    count++;
                
            }
            
            if(count <=mid){
                begin = mid+1;
            }else{
                end = mid-1;
            }
        }
        
        return begin;
        
    }
};
```

