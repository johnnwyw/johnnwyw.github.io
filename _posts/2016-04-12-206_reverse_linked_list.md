---
layout:     post
title:      "Reverse Linked List "
subtitle:   "链表翻转 "
date:       2016-04-12 11:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - leetcode
    - 链表
    - 数据结构
    - c++
    
---


### 206. Reverse Linked List

##### 解法一（递归方法）

```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL||head->next==NULL) 
            return head;  
          
        ListNode *p = head->next;  
        ListNode *cur = reverseList(p);  
          
        p->next = head;  
        head->next = NULL;  
        
        return cur; 
        
    }
};
```

##### 解法二（迭代方法）

```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL || head->next == NULL)
            return head;
        
        ListNode *pre = head;
        ListNode *p = head->next;
        ListNode *temp;
        pre->next = NULL;
        
        while(p != NULL){
            temp = p->next;
            p->next = pre;
            pre = p;
            p = temp;
        }
        return pre;
    }
};
```