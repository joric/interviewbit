# Remove Nth Element

https://www.interviewbit.com/problems/remove-nth-element


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