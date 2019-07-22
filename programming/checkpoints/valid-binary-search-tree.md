# Valid Binary Search Tree

https://www.interviewbit.com/problems/valid-binary-search-tree/

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
Example :

Input : 
```
   1
  /  \
 2    3
```

Output : 0 or False


Input : 
```
  2
 / \
1   3
```
Output : 1 or True 

Return 0 / 1 ( 0 for false, 1 for true ) for this problem


## Solution
```cpp
// editorial

/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
#define ll long long int
int isBST(TreeNode *a, ll min, ll max) {
    if (a == NULL)
        return 1;
    if (a->val <= min || a->val >= max)
        return 0;
    return isBST(a->left, min, a->val) && isBST(a->right, a->val, max);
}
int Solution::isValidBST(TreeNode *a) {
    if (a == NULL)
        return 0;
    return isBST(a, 1LL * (1LL * INT_MIN - 1), 1LL * (1LL * INT_MAX + 1));
}

/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
bool checkBST(TreeNode *root, int min, int max) {
    if (!root)
        return true;
    if (root->val <= min || root->val >= max)
        return false;
    return checkBST(root->left, min, root->val) && checkBST(root->right, root->val, max);
}

int Solution::isValidBST(TreeNode *A) {
    return checkBST(A, INT_MIN, INT_MAX);
}
```