# Add Two Numbers As Lists

https://www.interviewbit.com/problems/add-two-numbers-as-lists


## Solution

```cpp
ListNode* Solution::addTwoNumbers(ListNode* a, ListNode* b) {
    int carry = 0;
    ListNode * head = 0, * prev;
    while (a || b || carry) {
        int sum = (a ? a->val : 0) + (b ? b->val : 0) + carry;
        ListNode * node = new ListNode(sum % 10);
        carry = sum / 10;
        
        if (!head)
            head = prev = node;
        else {
            prev->next = node;
            prev = prev->next;
        }
        
        if (a)
            a = a->next;
        if (b)
            b = b->next;
    }
    return head;
}

```