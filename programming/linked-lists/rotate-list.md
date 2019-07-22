# Rotate List

https://www.interviewbit.com/problems/rotate-list

Given a list, rotate the list to the right by k places, where k is non-negative.

For example:

```
Given 1->2->3->4->5->NULL and k = 2,
return 4->5->1->2->3->NULL
```

## Hint 1

Note 1: n may be a number larger than the size of the list. 

Note 2: Try to figure out the steps for the rotation if you know the new start node, the actual end node and the node previous to the new start node.

## Solution Approach

Since n may be a large number compared to the length of list. So we need to know the length of linked list. After that, move the list after the (l-n % l )th node to the front to finish the rotation.

Ex: {1,2,3} k = 2 Move the list after the 1st node to the front

Ex: {1,2,3} k = 5, In this case Move the list after (3 - 5 % 3) 1st node to the front.

So the code has three parts.

1) Get the length

2) Move to the (l-n%l)th node

3) Do the rotation

## Solution

```cpp

/* editorial */

class Solution {
public:
    ListNode *rotateRight(ListNode *head, int k) {
        if (head == NULL || head->next == NULL)
            return head;

        ListNode *dummy = new ListNode(0);
        dummy->next = head;

        ListNode *fast = dummy, *slow = dummy;

        int sizeOfList = 0;
        while (fast->next != NULL) {
            fast = fast->next;
            sizeOfList++;
        }

        int firstNodePos = sizeOfList - (k % sizeOfList);
        for (int i = 0; i < firstNodePos; i++) {
            slow = slow->next;
        }

        fast->next = dummy->next;
        dummy->next = slow->next;
        slow->next = NULL;

        return dummy->next;
    }
};

/* lightweight */

ListNode *Solution::rotateRight(ListNode *head, int n) {
    int len = 1;
    ListNode *curr = head;

    // get the length
    while (curr->next) {
        curr = curr->next;
        len++;
    }

    // move to the (l - n%l)th node
    n %= len;
    curr->next = head;
    curr = head;
    for (int i = 0; i < len - n - 1; i++) {
        curr = curr->next;
    }

    //Do the rotation
    ListNode *res = curr->next;
    curr->next = 0;
    return res;
}

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
ListNode *Solution::rotateRight(ListNode *A, int B) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
    ListNode *curr = A;
    ListNode *head = A;
    ListNode *last;

    int sizeOv = 0;

    if (B == 0 || A == NULL) {
        return A;
    }

    while (curr != NULL) {
        last = curr;
        curr = curr->next;
        sizeOv++;
    }

    while (B >= sizeOv) {
        B = B - sizeOv;
    }

    B = sizeOv - B;

    last->next = head;
    // now, its a circular list

    curr = A;

    while (B != 0) {
        last = curr;
        curr = curr->next;
        B--;
    }

    head = curr;
    last->next = NULL;

    return head;
}
```