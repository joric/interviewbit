# Reverse Linked List

https://www.interviewbit.com/problems/reverse-linked-list/

Reverse a linked list. Do it in-place and in one-pass.

For example:

Given 1->2->3->4->5->NULL,

return 5->4->3->2->1->NULL.

## Solution

### Editorial
```cpp
ListNode *Solution::reverseList(ListNode *head) {
    if (head == NULL) return head;
    ListNode *cur = head, *nextNode, *prevNode;
    prevNode = NULL;

    while (cur != NULL) {
        nextNode = cur->next;
        cur->next = prevNode;
        prevNode = cur;
        cur = nextNode;
    }

    head = prevNode;
    return head;
}

```

### Mine
```cpp
ListNode* RecursiveReverse(ListNode* curr, ListNode* next, ListNode* prev){
    if (!curr)
        return prev;
    next = curr->next;
    curr->next = prev;
    RecursiveReverse(next, next, curr);
}

ListNode* Solution::reverseList(ListNode* A) {
    return RecursiveReverse(A, A->next, NULL);
}
```

