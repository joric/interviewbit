# Balanced Binary Tree

https://www.interviewbit.com/problems/balanced-binary-tree


## Solution

```cpp
int height(TreeNode *node) {
    if (!node)
        return 0;
    if (!node->left && !node->right)
        return 1;

    auto l = 1 + height(node->left);
    auto r = 1 + height(node->right);

    if (l < 0 || r < 0)
        return INT_MIN;
    if (abs(l - r) > 1)
        return INT_MIN;
    return max(l, r);
}
int Solution::isBalanced(TreeNode *A) {
    if (height(A) < 0)
        return 0;
    return 1;
}
```