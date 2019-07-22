# Sorted Array To Balanced Bst

https://www.interviewbit.com/problems/sorted-array-to-balanced-bst



Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

What will happen when you choose middle element of array as root?

Can you simulate the same thing for the left and right part of the array after that?

For a BST, all values lower than the root go in the left part of root, and all values higher go in the right part of the root. 
For the tree to be balanced, we will need to make sure we distribute the elements almost equally in left and right part. 
So we choose the mid part of the array as root and divide the elements around it .

Things to take care of: 
1) Are you passing a copy of the array around or are you only passing around a reference ? 
2) Are you dividing the array before passing onto the next function call or are you just specifying the start and end index ?


## Solution

```cpp

TreeNode * make_bst(const vector<int> &data, int i, int j) {
    if (i>j)
        return 0;
    int m = (i + j) / 2;
    TreeNode * node = new TreeNode(data[m]);
    node->left = make_bst(data, i, m - 1);
    node->right = make_bst(data, m + 1, j);
    return node;
 }
 
TreeNode* Solution::sortedArrayToBST(const vector<int> &A) {
    return make_bst(A, 0, A.size()-1);
}
```