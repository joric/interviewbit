# Partition List

https://www.interviewbit.com/problems/partition-list


Another pointer game. Traverse the list while maintaining two pointers( what should they represent?).

Maintain 2 pointers - one which maintains all nodes less than x and another which maintains
nodes greater than or equal to x. 
Keep going along our list. When we are at node that is greater than or equal to x, we remove
this node from our list and move it to list of nodes greater than x.

## Solution

```cpp

// editorial

ListNode *Solution::partition(ListNode *head, int x) {

    if (!head) return NULL;
    ListNode *curr = head;

    ListNode *start = new ListNode(0); // list of nodes greater than x
    ListNode *tail = start;

    ListNode *newHead = new ListNode(0);
    newHead->next = head;
    ListNode *pre = newHead; // previous node, we need it for removing

    while (curr) {
        if (curr->val >= x) {
            pre->next = curr->next; // remove from our list
            tail->next = curr;      // add to list of nodes greater than x
            tail = tail->next;
            curr = curr->next;
            tail->next = NULL;
        } else
            pre = curr, curr = curr->next;
    }
    pre->next = start->next;
    return newHead->next;
}

/////////////////////////////////////////////////////////

ListNode *Solution::partition(ListNode *head, int x) {
    if (!head || !head->next)
        return head;
    ListNode *first, *second, *f, *s;
    first = second = f = s = NULL;
    while (head) {
        if (head->val < x) {
            if (!first)
                f = first = head;
            else {
                first->next = head;
                first = first->next;
            }
        } else {
            if (!second)
                s = second = head;
            else {
                second->next = head;
                second = second->next;
            }
        }
        head = head->next;
    }
    if (s)
        second->next = NULL;

    if (f) {
        first->next = s;
        return f;
    }
    return s;
}
```