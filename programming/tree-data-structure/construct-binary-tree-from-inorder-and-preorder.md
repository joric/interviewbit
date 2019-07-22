# Construct Binary Tree From Inorder and Preorder

https://www.interviewbit.com/problems/construct-binary-tree-from-inorder-and-preorder


Given preorder and inorder traversal of a tree, construct the binary tree.

 Note: You may assume that duplicates do not exist in the tree. 
Example :

Input :
```
Preorder: [1, 2, 3]
Inorder : [2, 1, 3]
```

Return :
```
  1
 / \
2   3
```

First element of preorder traversal will be root. Combine this info with inorder traversal. How can this help you?


Focus on the preorder traversal to begin with. 
The first element in the traversal will definitely be the root. 
Based on this information, can you identify the elements in the left subtree and right subtree ?
( Hint: Focus on inorder traversal and root information )

Once you do that, your problem has now been reduced to a smaller set.
Now you have the inorder and preorder traversal for the left and right subtree and you need to figure them out. 
Divide and conquer.

Bonus points if you can do it without recursion.


## Solution

```cpp

// editorial

TreeNode *buildTreeTmp(vector<int>::iterator prel, vector<int>::iterator prer,
                       vector<int>::iterator inl, vector<int>::iterator inr) {
    if (prel >= prer)
        return NULL;

    int val = *prel;
    TreeNode *root = new TreeNode(val);

    vector<int>::iterator rootIndex = find(inl, inr, val);
    vector<int>::size_type lsize = rootIndex - inl;

    root->left = buildTreeTmp(prel + 1, prel + 1 + lsize, inl, rootIndex);
    root->right = buildTreeTmp(prel + 1 + lsize, prer, rootIndex + 1, inr);

    return root;
}

TreeNode *buildTree(vector<int> &preorder, vector<int> &inorder) {
    return preorder.size() == 0 ? NULL : buildTreeTmp(preorder.begin(), preorder.end(), inorder.begin(), inorder.end());
}

////////////////

// fastest

TreeNode *buildTreeUtil(vector<int> &preorder, vector<int> &inorder, int preStart, int preEnd, int inStart, int inEnd) {
    if (preStart > preEnd || inStart > inEnd) return nullptr;
    TreeNode *root = new TreeNode(preorder[preStart]);
    int k = 0;
    for (int i = inStart; i <= inEnd; i++) {
        if (inorder[i] == preorder[preStart]) {
            k = i;
            break;
        }
    }
    root->left = buildTreeUtil(preorder, inorder, preStart + 1, preStart + (k - inStart), inStart, k - 1);
    root->right = buildTreeUtil(preorder, inorder, preStart + (k - inStart + 1), preEnd, k + 1, inEnd);
    return root;
}

TreeNode *Solution::buildTree(vector<int> &preorder, vector<int> &inorder) {
    if (preorder.size() <= 0) return nullptr;
    return buildTreeUtil(preorder, inorder, 0, preorder.size() - 1, 0, inorder.size() - 1);
}

//////////////////

/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
int findIndex(vector<int> &inorder, int i, int j, int val) {
    for (auto b = i; b <= j; ++b)
        if (inorder[b] == val)
            return b;
}
TreeNode *makeTree(vector<int> &preorder, vector<int> &inorder, int i, int j, int &p) {
    if (i > j)
        return NULL;
    TreeNode *node = new TreeNode(preorder[p++]);
    if (i == j)
        return node;
    int in = findIndex(inorder, i, j, node->val);
    node->left = makeTree(preorder, inorder, i, in - 1, p);
    node->right = makeTree(preorder, inorder, in + 1, j, p);
    return node;
}
TreeNode *Solution::buildTree(vector<int> &preorder, vector<int> &inorder) {
    if (preorder.empty() || inorder.empty())
        return NULL;
    int p = 0;
    return makeTree(preorder, inorder, 0, inorder.size() - 1, p);
}
```