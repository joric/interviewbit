# Remove Nth Node from List End
https://www.interviewbit.com/problems/remove-nth-node-from-list-end/

Given a linked list, remove the nth node from the end of list and return its head.

For example,

Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.

Note:

If n is greater than the size of the list, remove the first node of the list.
Try doing it using constant additional space.

## Hint 1

Can you remove xth node from start instead of nth node from end? 
Can you figure out the position of node from start given its position from end?

## Solution Approach

Obviously, since we do not have back pointers, reaching the end node and then making our way back is not an option.

There are 2 approaches :

1) Find out the length of the list in one go. Then you know the number of node to be removed. Traverse to the node and remove it.

2) Make the first pointer go n nodes. Then move the second and first pointer simultaneously. This way, the first pointer is always ahead of the second pointer by n nodes. So when first pointer reaches the end, you are on the node to be removed.

## Solution

```cpp
ListNode* Solution::removeNthFromEnd(ListNode* A, int B) {
    ListNode *head = A, *a, *b, *prev = 0;
    
    for (a = head; a && B; a = a->next) {
        B--;
    }
    
    for (b = head; a && b; a = a->next) {
        prev = b;
        b = b->next;
    }
    
    if (prev && b) {
        prev->next = b->next;
    } else {
        head = head->next;  
    }
    
    return head;
}
```

## Asked in

* HCL
* Amazon
