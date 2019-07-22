# Merge Two Sorted Lists

https://www.interviewbit.com/problems/merge-two-sorted-lists

Merge two sorted linked lists and return it as a new list. 
The new list should be made by splicing together the nodes of the first two lists, and should also be sorted.

For example, given following linked lists :

5 -> 8 -> 20 

4 -> 11 -> 15

The merged list should be :

4 -> 5 -> 8 -> 11 -> 15 -> 20

## Hint 1
Maintain pointers in both the linked list and keep appending the elements to the list to be returned.

NOTE: You don't have to create new nodes here i.e. list to be returned should be made from the combination of both of the given lists.

## Solution Approach


First thing to note is that all you would want to do is modify the next pointers. You don't need to create new nodes.

At every step, you choose the minumum of the current head X on the 2 lists, and modify your answer's next pointer to X. You move the current pointer on the said list and the current answer.

Corner case, 
Make sure that at the end of the loop, when one of the list goes empty, you do include remaining elemnts from the second list into your answer.

Test case : 1->2->3 4->5->6


## Solution

```cpp
ListNode *Solution::mergeTwoLists(ListNode *A, ListNode *B) {
    ListNode *a = A, *b = B;
    if (A->val > B->val)
        swap(a, b);
    ListNode * head = a;
    while (a && b) {
        while (a->next && b && a->next->val < b->val)
            a = a->next;
        ListNode *tmp = a->next;
        a->next = b;
        a = b;
        b = tmp;
    }
    return head;
}

/* recursive */

ListNode *Solution::mergeTwoLists(ListNode *l1, ListNode *l2) {
    if (!l1)
        return l2;

    if (!l2)
        return l1;

    if (l1->val < l2->val) {
        l1->next = mergeTwoLists(l1->next, l2);
        return l1;
    }

    l2->next = mergeTwoLists(l1, l2->next);
    return l2;
}

/* editorial */

ListNode *Solution::mergeTwoLists(ListNode *l1, ListNode *l2) {
    if (l1 == NULL)
        return l2;
    if (l2 == NULL)
        return l1;

    ListNode *head = NULL; // head of the list to return

    // find first element (can use dummy node to put this part inside of the loop)
    if (l1->val < l2->val) {
        head = l1;
        l1 = l1->next;
    } else {
        head = l2;
        l2 = l2->next;
    }

    ListNode *p = head; // pointer to form new list

    while (l1 && l2) {
        if (l1->val < l2->val) {
            p->next = l1;
            l1 = l1->next;
        } else {
            p->next = l2;
            l2 = l2->next;
        }
        p = p->next;
    }

    // add the rest of the tail, done!
    if (l1) {
        p->next = l1;
    } else {
        p->next = l2;
    }

    return head;
}
```