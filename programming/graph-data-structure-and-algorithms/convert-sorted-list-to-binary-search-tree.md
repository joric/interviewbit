# Convert Sorted List To Binary Search Tree

https://www.interviewbit.com/problems/convert-sorted-list-to-binary-search-tree


Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

A height balanced BST : a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1. 

Example :

Given A : 1 -> 2 -> 3

A height balanced BST:

```
      2
    /   \
   1     3
```

## Hint 1

What will happen if you will make the middle element of linked list the root element of BST?

## Hint 2

Bruteforce :
Find the middle of the list, make it the root. Left part of the tree comes from the first half
and right part of the tree comes from the later half.

Can you think of making it better ? 
Note that you can construct the left tree first by passing the size of the left tree that you expect.

## Solution

```cpp

/////////////////////////////////

TreeNode *makeTree(ListNode *&head, int start, int end) {
    if (start > end)
        return 0;
    int mid = start + (end - start) / 2;
    TreeNode *left = makeTree(head, start, mid - 1);
    TreeNode *root = new TreeNode(head->val);
    head = head->next;
    TreeNode *right = makeTree(head, mid + 1, end);
    root->left = left;
    root->right = right;
    return root;
}

TreeNode *Solution::sortedListToBST(ListNode *A) {
    if (!A)
        return 0;
    int len = 0;
    for (ListNode *curr = A; curr; curr = curr->next)
        len++;
    return makeTree(A, 0, len - 1);
}

/////////////////////////////////
```