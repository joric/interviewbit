# Path Sum

https://www.interviewbit.com/problems/path-sum



Can you traverse the tree while keeping the sum from root to current node?

How can you check if you have reached the leaf or not?


Recursion might make this problem much easier to solve. 
You just need to keep a track of the sum from the root to the current node. 
Then it becomes a question of just checking if the current node is a leaf node, and if so, do the sum match.



## Solution

```cpp

// editorial

int Solution::hasPathSum(TreeNode *root, int sum) {
    if (!root) return false;
    if (!root->left && !root->right)
        return sum == root->val;
    int remainingSum = sum - root->val;
    return hasPathSum(root->left, remainingSum) || hasPathSum(root->right, remainingSum);
}

//////////////////////////////////////////////////////////

int pathsum(TreeNode* root, int sum, const int& target) {
    if (!root)
        return false;
    sum += root->val;
    if (!root->left && !root->right)
        return sum == target;
    return pathsum(root->left, sum, target) || pathsum(root->right, sum, target);
}
int Solution::hasPathSum(TreeNode* root, int target) {
    if (!root)
        return 0;
    return pathsum(root, 0, target);
}


```