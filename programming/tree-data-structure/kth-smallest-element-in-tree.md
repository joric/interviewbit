# Kth Smallest Element In Tree

https://www.interviewbit.com/problems/kth-smallest-element-in-tree


Given a binary search tree, write a function to find the kth smallest element in the tree.

Example :

Input: 
  2
 / \
1   3

and k = 2

Return: 2

As 2 is the second smallest element in the tree.
 NOTE: You may assume 1 <= k <= Total number of nodes in BST 
 

Think about the property of binary search tree and how it can help you.

Do you really need to visit right subtree of any node before visiting entire left subtree of it?


Note the property of the binary search tree. 
All elements smaller than root will be in the left subtree, and all elements greater than root will be in the right subtree. 
This means we need not even explore the right subtree till we have explored everything in the left subtree.
Or in other words, we go to the right subtree only when the size of left subtree + 1 ( root ) < k.

With that in mind, we can come up with an easy recursive solution which is similar to inorder traversal :

Step 1: Find the kth smallest element in left subtree decrementing k for every node visited.
If answer is found, return answer.
Step 2: Decrement k by 1. If k == 0 ( this node is the kth node visited ), return node's value
Step 3: Find the kth smallest element in right subtree.

## Solution

```cpp

// editorial

int find(TreeNode *root, int &k) {
    if (!root) return -1;
    // We do an inorder traversal here.
    int k1 = find(root->left, k);
    if (k == 0) return k1; // left subtree has k or more elements.
    k--;
    if (k == 0) return root->val; // root is the kth element.
    return find(root->right, k);  // answer lies in the right node.
}

int Solution::kthsmallest(TreeNode *root, int k) {
    return find(root, k); // Call another function to pass k by reference.
}

///////////

/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
void inorder(TreeNode *root, int &smallest, int k, int &it) {
    if (!root)
        return;
    inorder(root->left, smallest, k, it);
    ++it;
    if (it == k) {
        smallest = root->val;
        return;
    }
    inorder(root->right, smallest, k, it);
}
int Solution::kthsmallest(TreeNode *root, int k) {
    int smallest = -1;
    int it = 0;
    inorder(root, smallest, k, it);
    return smallest;
}
```