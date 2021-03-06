[Metadata]
title: Copy a singly linked list with a random pointer
date: 2014-08-16 14:44:58 

[Tags]
difficulty: 4
categories: linked-list, implementation
source: itint5, baidu

[Description]
Given you a singly linked list, and each node of the list contains a random pointer which points to arbitrary node of the list.

Your mission is to copy the list.

The defination of the node of the list is just like below.

```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode *random;
};
```

Hint:

* A solution which uses O(n) (or more) extra memory space will only get a score of 60.
* A solution which will destory the origin list will get a scroe of 80.
* A solution which takes only O(1) extra memory will receive full credit.

[Solution]
This problem is in level 3 if you take O(n) extra memory. And you make use of a std::map (or unordered_map in C++11) to map the origin nodes with the new ones.

The code below is easy to understand. And this solution is good enough for you to pass the interview of a company like Baidu. :)

```cpp
ListNode* copyListWithRandomPtr(ListNode *head) { // <= a solution of score 60
    unordered_map<ListNode*, ListNode*> mp;
    ListNode *ptr = head;
    ListNode *dummyHead = nullptr, *dummyPtr = nullptr;
    
    while (ptr) {
        if (dummyPtr) {
            dummyPtr -> next = new ListNode;
            dummyPtr = dummyPtr -> next;
        } else {
            dummyHead = dummyPtr = new ListNode;
        }
        *dummyPtr = *ptr;
        mp[ptr] = dummyPtr;
        ptr = ptr -> next;
    }
    
    dummyPtr = dummyHead;
    while (dummyPtr) {
        dummyPtr -> random = mp[dummyPtr -> random];
        dummyPtr = dummyPtr -> next;
    }
    return dummyHead;
}
```

The solution of score 80 is almost the best answer. It uses the ``next`` pointer to build the mapping. As it's not the best answer, and may give your interviewer a reason to doubt your answer, so I won't give the code here. 

The best solution is quite skillful. It changes the structure of the origin list, and devide it into two same list at last. Great answer for an interviewee to past the code test.

```cpp
ListNode* copyListWithRandomPtr(ListNode *head) {
    ListNode *ptr = head;
    ListNode *dummyHead = nullptr, *dummyPtr = nullptr;
    
    while (ptr) {
        auto next = ptr -> next;
        ptr -> next = new ListNode;
        ptr -> next -> next = next;
        ptr = next;
    }
    ptr = head;
    while (ptr) {
        auto next = ptr -> next -> next;
        auto random = ptr -> random ->next;
        ptr -> next -> random = random;
        ptr = next;
    }
    
    ptr = head;
    while (ptr) {
        auto next = ptr -> next -> next;
        auto dummy = ptr -> next;
        if (!dummyPtr) {
            dummyHead = dummyPtr = dummy;
        } else {
            dummyPtr -> next = dummy;
            dummyPtr = dummyPtr -> next;
        }
        ptr -> next = next;
        ptr = ptr -> next;
    }
    return dummyHead;
}
```
