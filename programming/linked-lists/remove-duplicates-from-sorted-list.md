# Remove Duplicates

https://www.interviewbit.com/problems/remove-duplicates-from-sorted-list


Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,

Given 1->1->2, return 1->2.

Given 1->1->2->3->3, return 1->2->3.

## Hint 1

You need to change next pointer of some element to next different element. Take care of corner cases if any.

## Solution Approach

Skip the node where head->next != NULL && head->val == head->next->val.

Make sure you take care of corner cases : 

1. Do you handle repetitions at the end ? ex : 1 -> 1
2. Do you handle cases where there is just one element ? ex : 1
3. Do you handle cases where there is just one element repeated numerous times ? 1->1->1->1->1->1


## Solution

```cpp

*
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x): val(x), next(NULL) {}
 * };
 */

/* editorial */

ListNode *deleteDuplicates(ListNode *head) {
	ListNode *origin = head;
	while (head != NULL) {
		while (head->next != NULL && head->val == head->next->val) {
			head->next = head->next->next;
		}
		head = head->next;
	}
	return origin;
}

/* mine */

ListNode *Solution::deleteDuplicates(ListNode *A) {
	ListNode *head = A;
	while (A) {
		if (A->next && A->next->val == A->val) {
			A->next = A->next->next;
		} else {
			A = A->next;
		}
	}
	return head;
}
```