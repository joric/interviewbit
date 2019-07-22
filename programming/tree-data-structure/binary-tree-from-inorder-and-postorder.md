# Binary Tree From Inorder and Postorder

https://www.interviewbit.com/problems/binary-tree-from-inorder-and-postorder


Given inorder and postorder traversal of a tree, construct the binary tree.

 Note: You may assume that duplicates do not exist in the tree. 
Example :

Input: 
        Inorder: [2, 1, 3]
        Postorder: [2, 3, 1]

Return: 
            1
           / \
          2   3

https://www.interviewbit.com/problems/construct-binary-tree-from-inorder-and-postorder/


Last element of postorder traversal will be root. Combine this info with inorder traversal. How can this help you?



Focus on the postorder traversal to begin with. 
The last element in the traversal will definitely be the root. 
Based on this information, can you identify the elements in the left subtree and right subtree ?
( Hint: Focus on inorder traversal and root information )

Once you do that, your problem has now been reduced to a smaller set. Now you have the inorder and postorder
traversal for the left and right subtree and you need to figure them out. 
Divide and conquer.

Bonus points if you can do it without recursion.



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

///////// editorial

TreeNode *create(vector<int> &inorder, vector<int> &postorder, int is, int ie, int ps, int pe) {
    if (ps > pe) {
        return NULL;
    }

    TreeNode *node = new TreeNode(postorder[pe]);
    int pos;
    for (int iter = is; iter <= ie; iter++) {
        if (inorder[iter] == node->val) {
            pos = iter;
            break;
        }
    }

    node->left = create(inorder, postorder, is, pos - 1, ps, ps + pos - is - 1);
    node->right = create(inorder, postorder, pos + 1, ie, pe - ie + pos, pe - 1);
    return node;
}

TreeNode *Solution::buildTree(vector<int> &inorder, vector<int> &postorder) {
    return create(inorder, postorder, 0, inorder.size() - 1, 0, postorder.size() - 1);
}

///////////////////

int findIndex(vector<int> &inorder, int i, int j, int val) {
    for (auto b = i; b <= j; ++b)
        if (inorder[b] == val)
            return b;
}

TreeNode *makeTree(vector<int> &inorder, vector<int> &postorder, int i, int j, int &p) {
    if (i > j)
        return NULL;
    TreeNode *node = new TreeNode(postorder[p--]);
    if (i == j)
        return node;
    int in = findIndex(inorder, i, j, node->val);
    node->right = makeTree(inorder, postorder, in + 1, j, p);
    node->left = makeTree(inorder, postorder, i, in - 1, p);
    return node;
}

TreeNode *Solution::buildTree(vector<int> &inorder, vector<int> &postorder) {
    int p = postorder.size() - 1;
    return inorder.empty() ? NULL : makeTree(inorder, postorder, 0, inorder.size() - 1, p);
}
```