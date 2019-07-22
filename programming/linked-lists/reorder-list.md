# Reorder List

https://www.interviewbit.com/problems/reorder-list


Given a singly linked list

    L: L0 -> L1 -> ... -> Ln-1 -> Ln,
reorder it to:

    L0 -> Ln -> L1 -> Ln-1 -> L2 -> Ln-2 -> ...
You must do this in-place without altering the nodes' values.

For example,
Given {1,2,3,4}, reorder it to {1,4,2,3}.

## Hint 1

Note that moving in the reverse order in the list is not feasible unless you use additional memory. This indicates that we need to reverse some part of the list.

Can you figure out a solution reversing a part of the list ?

## Hint 2

1) Break the list from middle into 2 lists.
2) Reverse the latter half of the list. 
3) Now merge the lists so that the nodes alternate.

## Solution

```cpp


/* editorial */

class Solution {
public:
    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
        if(l1 == NULL) return l2;
        if(l2 == NULL) return l1;

        ListNode* head = l1;    // head of the list to return
        l1 = l1->next;

        ListNode* p = head;     // pointer to form new list
        // A boolean to track which list we need to extract from. 
        // We alternate between first and second list. 
        bool curListNum = true;

        while(l1 && l2){
            if(curListNum == false) {
                p->next = l1;
                l1 = l1->next;
            } else {
                p->next = l2;
                l2 = l2->next;
            }
            p = p->next;
            curListNum = !curListNum;
        }

        // add the rest of the tail, done!
        if (l1) {
            p->next = l1;
        } else {
            p->next = l2;
        }

        return head;
    }

    ListNode *reverseLinkedList(ListNode *head) {
        if (head->next == NULL) return head;
        ListNode *cur = head, *nextNode = head->next, *tmp;

        while (nextNode != NULL) {
            tmp = nextNode->next;
            nextNode->next = cur;
            cur = nextNode;
            nextNode = tmp;
        }

        head->next = nextNode;
        return cur;
    }

    ListNode* reorderList(ListNode *head) {
        if(head == NULL || head->next == NULL || head->next->next==NULL)
            return head;

        //find the middle of the list, and split into two lists.    
        ListNode *slow = head,*fast = head;
        while(slow != NULL && fast != NULL && fast->next != NULL && fast->next->next != NULL){
            slow = slow->next;
            fast = fast->next->next;
        }

        ListNode *mid = slow->next;
        slow->next = NULL;

        //reverse from the middle to the end
        ListNode* secondHalfReversed = reverseLinkedList(mid);

        //merge these two list
        return head = mergeTwoLists(head, secondHalfReversed);
    }
};


/* another solution */


ListNode* Solution::reorderList(ListNode* A) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
    
    ListNode* temp;
    ListNode* prev;
    ListNode* mid = A;
    ListNode* curr = A;
    ListNode* newCurr;
    ListNode* newHead;
    ListNode* newTemp;
    ListNode* newPrev;
    
    if(A == NULL || A->next == NULL){
        return A;
    }
    
    while(curr != NULL && curr->next != NULL){
        prev = mid;
        mid = mid->next;
        curr = (curr->next)->next;
    }
    
    prev->next = NULL;
    
    newCurr = mid;
    
    while(newCurr != NULL){
        newTemp = newCurr->next;
        if(newCurr == mid){
            newPrev = newCurr;
            newCurr->next = NULL;
            newCurr = newTemp;
        }
        else{
            newCurr->next = newPrev;
            newPrev = newCurr;
            newCurr = newTemp;
        }
    }
    
    newHead = newPrev;
    newCurr = newHead;
    curr = A;
    
    while(newCurr != NULL && curr != NULL){
        prev = curr;
        newPrev = newCurr;
        temp = curr->next;
        newTemp = newCurr->next;
        
        curr->next = newCurr;
        if(temp != NULL){
            newCurr->next = temp;
        }
        curr = temp;
        newCurr = newTemp;
    }
    
    return A;
}
```