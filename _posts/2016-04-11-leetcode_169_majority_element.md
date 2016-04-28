---
layout:     post
title:      "Majority Element "
subtitle:   "出现次数大于数组长度一半的元素 "
date:       2016-04-11 12:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb1.jpg"
catalog:    true
tags:
    - leetcode
    - 数组
    - 数据结构
    - c++
    
---

### leetcode 169. Majority Element

##### 题意：
Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

##### 分析

此题有五种比较好的方法，分别是排序算法，随机算法，哈希算法，投票方法、位操作方法

1. 排序法（最简单）：将数组排序，然后输出中位数；时间复杂度: O(nlog n)
2. 随机方法：因为有大于50%的概率会随机抽中出现频率最大的数字，因此性能也算不错。具体方法就是在while循环中随机抽取一个数字，计算这个数字在数组中出现的次数，如果大于n/2次，那么就输出，不然的话就继续随机抽取下一个数字；
3. 哈希算法：维护一个每一个元素出现次数的哈希表, 然后找到出现次数最多的元素，时间复杂度: O(n), 空间复杂度: O(n)
4. 投票方法（最稳定，效率也很好）：这个算法的时间复杂度是O(n)，空间复杂度是O(1)，维护一个当前的候选众数candidate和一个初始为0的计数器count。遍历数组时，看当前的元素x。如果计数器是0, 我们将候选众数置为x 并将计数器置为 1，如果计数器非0, 我们根据x与当前的候选众数是否相等对计数器+1或者-1
5. 位操作方法：需要32次迭代, 每一次计算所有n个数的第i位的1的个数。由于众数一定存在，那么或者1的个数 > 0的个数 或者反过来(但绝不会相同)。 众数的第i位一定是计数较多数字。时间复杂度为O(n) 

#### 代码

##### 解法一（排序法）

```
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return nums[nums.size() / 2];
    }
};
```

##### 解法二（随机方法）

```
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();
        srand(unsigned(time(NULL)));
        while (true) {
            int index = rand() % n;
            int candidate = nums[index];
            int counts = 0;
            for (int i = 0; i < n; i++)
                if (nums[i] == candidate)
                    counts++;
            if (counts > n / 2)
                return candidate;
        }
    }
};
```

##### 解法三（哈希算法）

```
class Solution {
public:
    int majorityElement(vector<int> &num) {
        unordered_map<int, int> map;
        for (int i = 0; i < num.size(); i++)
        {
            map[num[i]]++;
        }
        unordered_map<int, int>::iterator itr;
        for (itr = map.begin();itr != map.end(); itr++)
        {
            if (itr -> second > num.size()/2)
                return itr -> first;
        }
    }
};
```

##### 解法四（投票方法）

```
class Solution {
public:
    int majorityElement(vector<int> &num) {
        int count = 0;
        int candidate = 0;
        for(int i = 0; i < num.size(); i ++)
        {
            if(count == 0)
            {
                candidate = num[i];
                nTimes = 1;
            }
            else
            {
                if(candidate == num[i])
                    count ++;
                else
                    count --;
            }
        }
        return candidate;
    }
};
```

##### 解法五（位操作方法）

```
class Solution {
public:
    int majorityElement(vector<int> &num) {
        int bitCount[32];
        memset(bitCount, 0, sizeof(bitCount));
        
        for (int i = 0; i < num.size(); i++)
        {
            for (int j = 0; j < 32; j++)
            {
                if (num[i] & (1 << j))
                    bitCount[j]++;
            }
        }
        
        int ans = 0;
        for (int i = 0; i < 32; i++)
        {
            if (bitCnt[i] > num.size()/2)
                ans += (int)pow(2, i);
        }
        return ans;
    }
};
```






