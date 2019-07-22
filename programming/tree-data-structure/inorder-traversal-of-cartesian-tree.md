# Inorder Traversal Cartesian Tree

https://www.interviewbit.com/problems/inorder-traversal-cartesian-tree


Given an inorder traversal of a cartesian tree, construct the tree.

 Cartesian tree: is a heap ordered binary tree, where the root is greater than all the elements in the subtree. 
 Note: You may assume that duplicates do not exist in the tree. 
Example :

Input: [1 2 3]

Return:   
          3
         /
        2
       /
      1
      
https://www.interviewbit.com/problems/inorder-traversal-of-cartesian-tree/


Given inorder traversal can you find the root here? What can you do once you get
the root element and its left and right subtree?


Inorder traversal: (Left tree) root (Right tree)

Note that the root is the max element in the whole array. Based on the info, can you figure out
the position of the root in inorder traversal ? If so, can you separate out the elements which
go in the left subtree and right subtree ? 
Once you have the inorder traversal for left subtree, you can recursively solve for left subtree.
Same for right subtree.


## Solution

```cpp

/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */



TreeNode *build(vector<int> &inorder, int start, int end) {
    if (start == end) {
        return new TreeNode(inorder[start]);
    }
    if (start > end) return NULL;

    // find max which will be the root. 
    int maxVal = INT_MIN, maxIndex = -1;
    for (int i = start; i <= end; i++) {
        if (inorder[i] > maxVal) {
            maxVal = inorder[i];
            maxIndex = i;
        }
    }

    TreeNode *root = new TreeNode(maxVal);
    root->left = build(inorder, start, maxIndex - 1);
    root->right = build(inorder, maxIndex + 1, end);
    return root;
}

TreeNode *Solution::buildTree(vector<int> &inorder) {
    if (inorder.size() == 0) return NULL;
    return build(inorder, 0, inorder.size() - 1);
}





//////////////////////////
int maxVal(int i, int j, vector<int>& A)
{
    int maxi = i;
    for (auto p = i+1; p<=j; ++p)
        if (A[maxi]<A[p])
            maxi = p;
    return maxi;
}

TreeNode* makeTree(int i, int j, vector<int>& A)
{
    if (i>j)
        return NULL;
    auto r = maxVal(i, j, A);
    TreeNode* node = new TreeNode(A[r]);
    node->left = makeTree(i, r-1, A);
    node->right = makeTree(r+1, j, A);
    return node;
}

TreeNode* Solution::buildTree(vector<int> &A) {
    if (A.empty())
        return NULL;
    return makeTree(0, A.size()-1, A);
}


```