---
layout:     post
title:      "排序算法"
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

### 3. 对排序

```

#include <iostream>
#include <vector>

using namespace std;


//堆筛选函数，调整为一个大顶堆
typedef int ElemType;
void heapAdjust(vector<ElemType> &nums, int start, int end)
{
    
    ElemType temp = nums[start];
    //因为假设根结点的序号为0而不是1，所以i结点左孩子为2i+1
    int i = 2*start+1;
    
    while(i<=end)
    {
        if(i<end && nums[i]<nums[i+1])//左右孩子的比较
        {
            i++;//i为较大的记录的左孩子下标
        }
        
        if(temp > nums[i])//左右孩子中较大者与父节点比较
        {
            break;
        }
        
        //将孩子结点赋值给父节点，以孩子结点的位置进行下一轮的筛选
        nums[start]= nums[i];
        start = i;
        i = 2*i + 1;
        
    }
    
    nums[start]= temp; //插入最开始不和谐的节点
}

void heapSort(vector<int> &nums,int n)
{
    //1.建立大顶堆
    for(int i=(n/2-1); i>=0; i--)
    {
        heapAdjust(nums,i,n-1);
    }
    //2.进行排序
    for(int i=n-1; i>0; --i)
    {
        //最后一个元素和第一元素进行交换
        ElemType temp=nums[i];
        nums[i] = nums[0];
        nums[0] = temp;
        
        //剩下的无序元素继续调整为大顶堆

        heapAdjust(nums,0,i-1);
    }
    
}



int main(int argc, const char * argv[]) {
    vector<ElemType> nums= {1,2,5,8,7,55,10};
    
    
 
    heapSort(nums,(int)nums.size());
    
    cout<<"after sorted:"<<endl;
    for(int j = 0;j < 7;j++){
        cout<<nums[j]<<", ";
    }
    
    cout<<endl;
    
    return 0;
}

```



