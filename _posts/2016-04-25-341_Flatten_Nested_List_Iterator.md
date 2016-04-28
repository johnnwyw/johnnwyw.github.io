---
layout:     post
title:      "Flatten Nested List Iterator "
subtitle:   "  "
date:       2016-04-24 09:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - DFS
    - 数据结构
    - c++
    
---


### 341. Flatten Nested List Iterator

#### 题意

Given a nested list of integers, implement an iterator to flatten it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Example 1:
Given the list [[1,1],2,[1,1]],

By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,1,2,1,1].

Example 2:
Given the list [1,[4,[6]]],

By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,4,6].

#### 思路

**DFS（Depth-First-Search）深度优先搜索算法**

1. 用一个vector将所有的值都先存起来
2. 设置一个索引标记
3. 调用next时，则返回数组vector的下一个值
4. 调用hasNext时，则判断index是否超出了vector数组的大小.

##### 代码

```
class NestedIterator {
    
private:
    int index;
    vector<int> vec;
public:
    
    void DFS(vector<NestedInteger> &nestedList)
    {
        for(auto val: nestedList)
        {
            if(val.isInteger())
                vec.push_back(val.getInteger());
            else {
                vector<NestedInteger> tmp=val.getList();
                DFS(tmp);
            }
        }
    }
    NestedIterator(vector<NestedInteger> &nestedList) {
        index = 0;
        DFS(nestedList);
    }
    
    int next() {
        return vec[index++];
    }
    
    bool hasNext() {
        return index < vec.size();   
    }
};

```

