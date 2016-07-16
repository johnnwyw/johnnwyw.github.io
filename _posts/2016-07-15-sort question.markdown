---
layout:     post
title:      "Binary Tree Level Traversal "
subtitle:   "  "
date:       2016-07-15 09:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - c++
    - 数据结构
    - 排序
  
    
---

## 排序算法

### 1. 快速排序

```

#include <iostream>
#include <vector>
using namespace std;

void quickSort(vector<int> &nums,int start,int end){
    int i = start,j = end,temp = nums[start],t;
    
    if(start > end){
        return;
    }
    
    temp = nums[start];
    while(i != j){
        while(nums[j] >= temp && i < j){
            j--;
        }
        while(nums[i] <= temp && i < j){
            i++;
        }
        
        if(i < j){
            t = nums[i];
            nums[i] = nums[j];
            nums[j] = t;
        }
    }
    
    nums[start] = nums[i];
    nums[i] = temp;
    
    quickSort(nums, start, i-1);
    quickSort(nums, i+1, end);
}

int main(int argc, const char * argv[]) {
    
    vector<int> nums = {1,2,5,8,7,4,9,11,19,18,16};
    
    
    cout<<"before sorted:"<<endl;
    vector<int>::iterator it = nums.begin();
    
    while(it != nums.end()){
        cout<<*it<<", ";
        it++;
    }
    cout<<endl;
    quickSort(nums, 0, (int)nums.size()-1);
    
    cout<<"after sorted:"<<endl;
    for(int j = 0;j < nums.size();j++){
        cout<<nums[j]<<", ";
    }
    
    cout<<endl;
    return 0;
}
```

### 2.归并排序

```

#include <iostream>
#include <vector>
using namespace std;

void mergeArray(vector<int> &nums,int begin,int mid,int end){
    int i = begin,j = mid+1,index = 0;
    int * temp = new int[end - begin +1];
    while(i<=mid && j <=end){
        if(nums[i] >= nums[j]){
            temp[index++] = nums[j++];
        }
        else{
            temp[index++] = nums[i++];
        }
    }
    while(i <= mid){
        temp[index++] = nums[i++];
    }
    while(j <= end){
        temp[index++] = nums[j++];
    }
    
    for(int i = 0;i<index;i++){
        nums[begin +i] = temp[i];
    }
}

void mergeSort(vector<int> &nums,int begin,int end){
    if(begin >= end){
        return;
    }
    int mid = begin + ((end - begin)>>1);
    
    mergeSort(nums, begin, mid);
    mergeSort(nums, mid+1, end);
    
    mergeArray(nums,begin,mid,end);
    
}

int main(int argc, const char * argv[]) {
    vector<int> nums = {1,2,5,8,7,4,9,11,19,18,16};
    
    
    cout<<"before sorted:"<<endl;
    vector<int>::iterator it = nums.begin();
    
    while(it != nums.end()){
        cout<<*it<<", ";
        it++;
    }
    cout<<endl;
    mergeSort(nums, 0, (int)nums.size()-1);
    
    cout<<"after sorted:"<<endl;
    for(int j = 0;j < nums.size();j++){
        cout<<nums[j]<<", ";
    }
    
    cout<<endl;

    return 0;
}

```



