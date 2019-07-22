# Palindrome List

https://www.interviewbit.com/problems/palindrome-list


Given a singly linked list, determine if its a palindrome. Return 1 or 0 denoting
if its a palindrome or not, respectively.
Notes:
Expected solution is linear in time and constant in space.

We need to check if first half is equal to last half(when reversed). But you can not store
different copy of reversed last half as this solution will not have constant space.
Can you modify the original list to do the above task?

To check palindrome, we just have to reverse latter half of linked list and then we can in (n/2) steps total
can compare if all elements are same or not.

For finding mid point, first we can in O(N) traverse whole list and calculate total number of elements.
Reversing is again O(N).


## Solution

```cpp


ListNode* reverseSubList(ListNode* A, int B){
  ListNode *next, *current, *prev;
  int temp = B;
  prev = A;
  while(temp-- && prev != NULL){
    prev = prev -> next;
  }
  current = A;
  while (current != NULL && B--) {
    next = current -> next;
    current -> next = prev;
    prev = current;
    current = next;
  }
  return prev;
}


int Solution::lPalin(ListNode* A) {
  ListNode* temp = A;
  int count = 0;
  while (temp != NULL) {
    count++;
    temp = temp -> next;
  }
  int n = count/2;
  A = reverseSubList(A, n);
  temp = A;
  while(n--){
    temp = temp->next;
  }
  if (count % 2 == 1) {
    temp = temp -> next;
  }
  while(temp!= NULL){
    if (A->val != temp->val) {
      return 0;
    }
    A = A->next;
    temp = temp->next;
  }
  return 1;   
}


#if 0
int Solution::lPalin(ListNode* A) {
    if (!A || !A->next)
        return 1;
    stack<int> s;
    ListNode* slow = A, *fast = A;
    while (fast && fast->next) {
        s.push(slow->val);
        slow = slow->next;
        fast = fast->next->next;
    }
    
    if (fast)
        slow = slow->next;
    
    while (s.size() && slow)
    {
        if (s.top() != slow->val)
            return 0;
        s.pop();
        slow = slow->next;
    }
    return 1;
}
#endif

```