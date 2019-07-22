# Sort List

https://www.interviewbit.com/problems/sort-list


Sort a linked list in O(n log n) time using constant space complexity.
Example :
Input: 1 -> 5 -> 4 -> 3
Returned list: 1 -> 3 -> 4 -> 5
## Solution

```cpp

ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
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

ListNode* Solution::sortList(ListNode *head) {
    if (head == NULL || head->next == NULL)
        return head;

    // find the middle place
    ListNode *p1 = head;
    ListNode *p2 = head->next;
    while (p2 && p2->next) {
        p1 = p1->next;
        p2 = p2->next->next;
    }
    p2 = p1->next;
    p1->next = NULL;

    return mergeTwoLists(sortList(head), sortList(p2));
}


#if 0

ListNode* merge(ListNode* A, ListNode* B){
    if(A == NULL){
        return B;
    }
    if(B == NULL){
        return A;
    }
    
    ListNode* head = NULL;
    
    if(A->val < B->val){
        head = A;
        A = A->next;
    }
    else{
        head = B;
        B = B->next;
    }
    
    ListNode* temp = head;
    
    while(A != NULL && B != NULL){
        if(A->val < B->val){
            temp->next = A;
            A = A->next;
        }
        else{
            temp->next = B;
            B = B->next;
        }
        temp = temp->next;
    }
    
    if(A != NULL){
        temp->next = A;
    }
    else{
        temp->next = B;
    }
    
    return head;
} 

ListNode* Solution::sortList(ListNode* A) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details

    ListNode* head = A;
    
    if(head == NULL || head->next == NULL){
        return head;
    }
    
    ListNode* start = A;
    ListNode* end = A->next;
    
    while(end != NULL && end->next != NULL){
        start = start->next;
        end = (end->next)->next;
    }
    
    end = start->next;
    start->next = NULL;
    
    return merge(sortList(head), sortList(end)); 
}

#endif
```