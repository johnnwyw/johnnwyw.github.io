---
layout:     post
title:      "leetcode328. Odd Even Linked List "
subtitle:   "  "
date:       2016-04-27 12:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - List
    - leetcode
    - 数据结构
    - c++
    
---


### leetcode328. Odd Even Linked List

#### 题意

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

**Example:**

Given 1->2->3->4->5->NULL,

return 1->3->5->2->4->NULL.

***Note:***

The relative order inside both the even and odd groups should remain as it was in the input. <br>
The first node is considered odd, the second node even and so on ...

####  代码

```
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if (!head) return head;
        struct ListNode *even, *p, *q;
        p = head;
        q = even = head->next;
        while (q && q->next){
            p = p->next = q->next;
            q = q->next = p->next;
        }
        p->next = even;
        return head;
        
    }
};
```


