# Max Sum Path in Binary Tree

https://www.interviewbit.com/problems/max-sum-path-in-binary-tree/

Given a binary tree, find the maximum path sum.

The path may start and end at any node in the tree.

Example :

Given the below binary tree,
```
       1
      / \
     2   3
```
Return 6.

## Hint 1

This is a classical DP on tree problem.

Can you try to compute the answer for any vertex if you have answer for their left and right child?

## Solution Approach

There are several ways of looking at this problem. 

If we knew that root R is going to be a part of the longest path. Then we can look at the longest path to any leaf in the left subtree, longest path in the right subtree, and add them up along with root's value to get the answer ( Obviously we only consider a path if its value is not negative ). To calculate the longest path till a leaf is O(n) [ Recursive call carrying forward the cumulative sum to a node ]. 

Given N possible roots, and then the O(n) leaf path calculation, the bruteforce becomes O(n^2).

However, note that the calculation of the longest path to the leaf is very redundant. So, to calculate the result for root R, can you reuse the results you have for R->left / R->right ?

## Solution

### Editorial
```cpp
int maxPathAndGlobalUpdate(TreeNode *root, int &global_max) {
    if (root == NULL) return 0;
    int l = max(0, maxPathAndGlobalUpdate(root->left, global_max));
    int r = max(0, maxPathAndGlobalUpdate(root->right, global_max));
    global_max = max(global_max, l + r + root->val);
    return root->val + max(l, r);
}

int Solution::maxPathSum(TreeNode *root) {
    int maxAns = INT_MIN;
    maxPathAndGlobalUpdate(root, maxAns);
    return maxAns;
}

```

### Fastest
```cpp
int func(TreeNode *a, int &res) {
    if (a == NULL)
        return 0;
    int l = func(a->left, res);
    int r = func(a->right, res);
    int ms = max(max(l, r) + a->val, a->val);
    int mtop = max(ms, l + r + a->val);
    res = max(res, mtop);
    return ms;
}

int Solution::maxPathSum(TreeNode *A) {
    int res = INT_MIN;
    func(A, res);
    return res;
}

```

## Asked in
* Directi
* Amazon
