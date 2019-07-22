# Least Common Ancestor

https://www.interviewbit.com/problems/least-common-ancestor



Pick every node. For every node, search for val1, val2 in the subtree. If val1 and val2 are both found in the subtree, then the current node is definitely one of the ancestors. Also track the depth of the current node. Pick the qualifying node of highest depth.

Hint for a better solution :

1) If you had the path from the nodes to the root, what property would the path have ? Can the paths be used to determine LCA ? 
2) If you took bottom up approach using recursion, can you think of a simple solution ?

//////////////////

Linear solution using path calculation :

1) Find path from root to n1 and store it in a vector or array.
2) Find path from root to n2 and store it in another vector or array.
3) Traverse both paths till the values in arrays are same. Return the common element just before the mismatch

Linear solution using recursion :

We traverse from the bottom, and once we reach a node which matches one of the two nodes,
we pass it up to its parent. The parent would then test its left and right subtree
if each contain one of the two nodes. If yes, then the parent must be the LCA and 
we pass its parent up to the root. If not, we pass the lower node which contains either
one of the two nodes (if the left or right subtree contains either p or q), or NULL
(if both the left and right subtree does not contain either p or q) up.

//////////////////////
## Solution

```cpp

// editorial

TreeNode *LCA(TreeNode *root, int val1, int val2) {
    if (!root) return NULL;
    if (root->val == val1 || root->val == val2) return root;
    TreeNode *L = LCA(root->left, val1, val2);
    TreeNode *R = LCA(root->right, val1, val2);
    if (L && R) return root;
    return L ? L : R;
}

bool find(TreeNode *root, int val1) {
    if (!root) return false;
    if (root->val == val1) return true;
    return find(root->left, val1) || find(root->right, val1);
}

int Solution::lca(TreeNode *root, int val1, int val2) {
    if (!find(root, val1) || !find(root, val2)) return -1;
    TreeNode *ans = LCA(root, val1, val2);
    if (!ans) return -1;
    return ans->val;
}

// fastest

bool find(TreeNode *a, int v, vector<int> &v1) {
    if (!a) return false;
    v1.push_back(a->val);
    if (a->val == v) return true;
    if (((a->left) && find(a->left, v, v1)) || (a->right && find(a->right, v, v1))) return true;
    v1.pop_back();
    return false;
}

int Solution::lca(TreeNode *a, int val1, int val2) {
    vector<int> v1, v2;
    if (!find(a, val1, v1) || !find(a, val2, v2)) return -1;
    int i;
    for (i = 0; i < int(v1.size()) && i < int(v2.size()); i++)
        if (v1[i] != v2[i]) break;
    return v1[i - 1];
}

/////////////////////

bool find(TreeNode *root, int val) {
    if (!root)
        return false;
    if (root->val == val)
        return true;
    if ((root->left && find(root->left, val)) ||
        (root->right && find(root->right, val)))
        return true;
    return false;
}

TreeNode *LCA(TreeNode *root, int val1, int val2) {
    if (!root || root->val == val1 || root->val == val2)
        return root;

    auto L = LCA(root->left, val1, val2);
    auto R = LCA(root->right, val1, val2);

    if (L && R)
        return root;
    return L ? L : R;
}

int Solution::lca(TreeNode *A, int val1, int val2) {
    if (!find(A, val1) || !find(A, val2))
        return -1;
    auto ancestor = LCA(A, val1, val2);
    if (ancestor)
        return ancestor->val;
    return -1;
}
```