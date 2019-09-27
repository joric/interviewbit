# Reverse Link List Recursion

https://www.interviewbit.com/problems/reverse-link-list-recursion/

Reverse a linked list using recursion.

Example :
Given 1->2->3->4->5->NULL,

return 5->4->3->2->1->NULL.

### PROBLEM APPROACH

Complete solution code in the hints

#### Video

[![](http://img.youtube.com/vi/KYH83T4q6Vs/0.jpg)](https://youtu.be/KYH83T4q6Vs)


## Solution
### Editorial
```cpp
class Solution {
        public:

            // Reverses the linkedList which starts from head
            // Returns the end node which is the head of the reversed list.  
            ListNode *reverseList(ListNode *p) {
                if (p->next == NULL || p == NULL) {
                    return p;
                }
                ListNode *nextNode = p->next;
                ListNode *head = reverseList(p->next);
                nextNode->next = p;
                p->next = NULL;

                return head;
            }
    };
```

### Fastest
```cpp
void reverselist(ListNode *previous, ListNode *current, ListNode*& head) {
    //base case
    if(current==NULL){
        head = previous;
        return;
    }
    reverselist(current, current->next, head);
    current->next = previous;
    return;
}
 
ListNode* Solution::reverseList(ListNode* A){
    ListNode *head = NULL;
    reverselist(NULL, A, head);
}
```

### Lightweight
```cpp
ListNode* Solution::reverseList(ListNode* A) {
    if(A->next==NULL){
        return A;
    }
    
    ListNode *rev=reverseList(A->next);
    A->next->next=A;
    A->next=NULL;
    return rev;
}
```

### Mine
```cpp
ListNode* RecursiveReverse(ListNode* curr, ListNode* next, ListNode* prev){
    if (!curr) return prev;
    next = curr->next;
    curr->next = prev;
    RecursiveReverse(next, next, curr);
}

ListNode* Solution::reverseList(ListNode* A) {
    return RecursiveReverse(A, A->next, NULL);
}
```

### Another Mine
```
ListNode* Solution::reverseList(ListNode* A) {
    if (!A->next) return A;
    ListNode * tail = reverseList(A->next);
    A->next->next = o;
    A->next = NULL;
    return tail;
}
```
