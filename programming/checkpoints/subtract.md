# SUBTRACT

https://www.interviewbit.com/problems/subtract/

Given a singly linked list, modify the value of first half nodes such that :

1. 1st node's new value = the last node's value - first node's current value
2. 2nd node's new value = the second last node's value - 2nd node's current value,

and so on ...

### NOTE

If the length L of linked list is odd, then the first half implies at first floor(L/2) nodes. So, if L = 5, the first half refers to first 2 nodes.
If the length L of linked list is even, then the first half implies at first L/2 nodes. So, if L = 4, the first half refers to first 2 nodes.


Example :

Given linked list 1 -> 2 -> 3 -> 4 -> 5,

You should return 4 -> 2 -> 3 -> 4 -> 5
as
```
for first node, 5 - 1 = 4
for second node, 4 - 2 = 2
```
Try to solve the problem using constant extra space.

## Solution

```cpp
ListNode *revert(ListNode *start) {
    if (!start)
        return NULL;

    ListNode *n1 = start;
    ListNode *n2 = n1->next;
    ListNode *n3;

    while (n2) {
        n3 = n2->next;
        n2->next = n1;
        n1 = n2;
        n2 = n3;
    }
    start->next = NULL;

    return n1;
}

ListNode *Solution::subtract(ListNode *A) {
    if (!A)
        return NULL;
    int len = 0;

    for (ListNode *n = A; n; n = n->next)
        len++;
    if (len < 2)
        return A;

    int halfLen = len / 2 + len % 2;
    ListNode *A2 = A;
    for (int i = 0; i < halfLen; i++)
        A2 = A2->next;

    ListNode *rear = revert(A2);
    ListNode *tf = A, *tr = rear;
    int halfLen2 = len / 2;

    for (int i = 0; i < halfLen2; i++) {
        tf->val = tr->val - tf->val;
        tf = tf->next;
        tr = tr->next;
    }

    revert(rear);
    return A;
}
```
