# Swap List Nodes in pairs

https://www.interviewbit.com/problems/swap-list-nodes-in-pairs/

Given a linked list, swap every two adjacent nodes and return its head.

For example,

Given 1->2->3->4, you should return the list as 2->1->4->3.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

## Hint 1

A->B->C.

Take a look at A,B. How can you change the next pointer of these elements without changing their values such that they are swapped?

## Solution Approach

Lets first look at the problem of swapping 2 nodes.

Method 1: Just swap the values in the 2 nodes. In most cases, this won't be a permissible solution.

Method 2: Move around the pointers.

## Solution

```cpp
ListNode* Solution::swapPairs(ListNode* A) {
    if (!A)
        return NULL;
    ListNode * head = A, * curr = A, * next = A->next, * prev = 0;
    if (next)
        A = next;
    while (next) {
        curr->next = next->next;
        next->next = curr;
        if(prev)            
            prev->next = next;
        prev = curr;
        curr = curr->next;
        if(curr)
            next = curr->next;
        else
            next = 0;
    }
    return A;
}
```

## Asked in

* Microsoft
* Amazon

