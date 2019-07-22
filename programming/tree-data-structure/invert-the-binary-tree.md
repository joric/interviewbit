# Invert The Binary Tree

https://www.interviewbit.com/problems/invert-the-binary-tree


## Solution

```cpp
TreeNode* Solution::invertTree(TreeNode* A) {
    if (!A)
        return 0;
    TreeNode * left = invertTree(A->left);
    TreeNode * right = invertTree(A->right);
    A->left = right;
    A->right = left;
    return A;
}

```