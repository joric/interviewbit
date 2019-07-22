# Insertion Sort List

https://www.interviewbit.com/problems/insertion-sort-list



## Hint 1

If you know about the insertion sort, then the key step here is swapping two adjacent nodes.

## Hint 2

This is very much a simulation problem.

The only trick is how do you move a node from ith position to jth position. 
How do you move the pointers to do so ? Would having a temporary node help ?


## Solution

```cpp

ListNode *Solution::insertionSortList(ListNode *head) {
    if (!head || !head->next)
        return head;
    ListNode *sorted = 0;

    while (head) {
        ListNode *curr = head;
        head = head->next;

        if (!sorted || sorted->val >= curr->val) {
            curr->next = sorted;
            sorted = curr;
        } else {
            ListNode *temp = sorted;
            while (temp) {
                ListNode *s = temp;
                temp = temp->next;

                if (!s->next || s->next->val > curr->val) {
                    curr->next = s->next;
                    s->next = curr;
                    break;
                }
            }
        }
    }
    return sorted;
}
```