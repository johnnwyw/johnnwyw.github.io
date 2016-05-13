---
layout:     post
title:      "leetcode 202. Happy Number   "
subtitle:   "  "
date:       2016-05-13 08:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - 数据结构
    - c++
  
    
---


### leetcode 202. Happy Number  

#### 题意

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

**Example:** 19 is a happy number

$1^{2} + 92 = 82

82 + 22 = 68

62 + 82 = 100

12 + 02 + 02 = 1


##### 分析

* 同计算循环小数一样, 如果出现循环, 则无需继续计算,直接返回false即可. 
* 否则循环计算每位数字的平方和，直到出现结果为1

##### 代码(递归解法)

```

class Solution {
     
private:
    long digitSquare(long num) {  
        
        long sum = 0;  
        while(num!=0) {  
            sum += pow(num%10, 2);  
            num /= 10;  
        }  
        return sum;  
    }  
public:
    
    bool isHappy(int n) {
        unordered_map<int,bool> res; 
        if(n<=0) 
            return false;  
        n = digitSquare(n); 
        while(n != 1) {  
            if(res[n] == true) 
                return false; 
                
            res[n] = true;  
            n = digitSquare(n);  
           
        }  
        return true;  
        
    }
    
    
};

```
