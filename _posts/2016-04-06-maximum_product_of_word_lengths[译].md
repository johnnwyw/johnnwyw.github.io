---
layout:     post
title:      "Maximum Product of Word Lengths "
subtitle:   "单词长度的最大积 "
date:       2016-04-05 08:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb1.jpg"
catalog:    true
tags:
    - leetcode
    - 位运算
    - 数据结构
    - c++
    
---


###  318. Maximum Product of Word Lengths[译]


##### 题意


Given a string array words, find the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.

##### Example

Example 1:
Given ["abcw", "baz", "foo", "bar", "xtfn", "abcdef"]
Return 16
The two words can be "abcw", "xtfn".

Example 2:
Given ["a", "ab", "abc", "d", "cd", "bcd", "abcd"]
Return 4
The two words can be "ab", "cd".

Example 3:
Given ["a", "aa", "aaa", "aaaa"]
Return 0
No such pair of words.


#### 解法一

##### 分析

题目中说都是小写字母，那么只有26位，一个整型数int有32位，可以用后26位来对应26个字母，若为1，说明该对应位置的字母出现过，那么每个单词的都可由一个int数字表示，两个单词没有共同字母的条件是这两个int数进行与操作后结果为0

##### 代码

```
class Solution{
public:
    int maxProduct(vector<string>& words){
       	int size = (int)words.size();
        vector<int>mask(size,0);
        int res = 0;
        for(int i = 0;i<size;i++){
            for(char c:words[i])
                mask[i] |= 1 << (c-'a');
            
            for(int j = 0;j < i;j++){
                if(!(mask[i]&mask[j])){
                    res = max(res,int(words[i].size()*words[j].size()));
                }
            }
            
        }
        return res;
    }
};
```

#### 解法一

##### 分析

借助哈希表，映射每个mask的值和其单词的长度，每算出一个单词的mask，如果其值（单词长度）大于当前值，就更新哈希表里的值，然后遍历哈希表，如果当前单词的mask值和哈希表中的mask值进行与操作后结果为0，则将当前单词的长度和哈希表中存的单词长度相乘并更新结果。

#####  代码

```
class Solution{
public:
    int maxProduct(vector<string>& words){
        int res = 0;
        map<int, int> map;
        for(auto word:words){
            int mask = 0;
            for(char c:word)
                mask = 1 << (c-'a');
            map[mask] = max(map[mask],int(word.size()));
            for(auto a:map){
                if(!(a.first)&mask)
                    res = max(res,int(word.size()*a.second));
            }
                
        }
        
        return res;
    }
};
```

[参考资料](http://www.cnblogs.com/grandyang/p/5090058.html)
