---
layout:     post
title:      " Roman Integer "
subtitle:   " 罗马数和整形的相互转换 "
date:       2016-04-18 12:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - 进制
    - 数据结构
    - c++
    
---


### 罗马数和整形的相互转换

#### 1、Roman to Integer

Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.


##### 代码

```
class Solution {
public:
    int romanToInt(string s) {
        int res = toDecimal(s[0]);
        
        for(int i = 1;i<s.length();i++){
            if(toDecimal(s[i]) <=toDecimal(s[i-1]))
                res += toDecimal(s[i]);
            else
                res += toDecimal(s[i]) - 2*toDecimal(s[i-1]);
        }
        
        
        return res;
       
    }
    int toDecimal(char c){
        switch (c) {
            case 'I':
                return 1;
            case 'V':
                return 5;
            case 'X':
                return 10;
            case 'L':
                return 50;
            case 'C':
                return 100;
            case 'D':
                return 500;
            case 'M':
                return 1000;
                
            default:
                return 0;
        }
    }
};

```

###  2、 Integer to Roman 

Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.

#####   代码

##### 解法一

```
class Solution {
public:
    string intToRoman(int num) {
        string  roman[4][10] = {
            {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"},
            {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"},
            {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"},
            {"", "M", "MM", "MMM"}
        };
        
        string res = "";
        int index = 0;
        while (num != 0) {
            int digit = num % 10;
            res = roman[index][digit] + res;
            index++;
            num /= 10;
        }
        
        return res;
        
    }

};
```

##### 解法二（打表）

```
class Solution {
public:
    string intToRoman(int num) {
        string str;    
        string table[13]={"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"};    
        int value[13]=    {1000,900,500,400, 100, 90,  50, 40,  10, 9,   5,  4,   1};   
        for(int i=0;num!=0;i++)  
        {  
            while(num>=value[i])  
            {  
                num-=value[i];  
                str+=table[i];  
            }  
        }  
        return str;  
        
    }

};
```