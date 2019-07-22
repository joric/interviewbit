# Root to Leaf Paths With Sum

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

For example:
Given the below binary tree and sum = 22,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```
return
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```
## Hint 1

Hint : Think recursion.

What if in your recursive function, you already know about the sum till the current node.

GOTCHAS:

If you are using C++, make sure to pass the array by reference.

## Solution Approach

Recursion might make this problem much easier to solve.

You just need to keep a track of : 
1) the sum from the root to the current node. 
2) The elements encountered from the root to this node.

Then it becomes a question of just checking if the current node is a leaf node, and if so, do the sum match.
If sums match, then you append the current elements from root to this node to the answer list.

Check for a node being a leaf:

```
node->left == NULL && node->right == NULL
```

## Solution

```cpp
void rootleaf( TreeNode *pvar, int sum, vector<vector<int>> &ans, vector<int> curr ) {
    if(!pvar)
        return;
    curr.push_back( pvar->val );
    rootleaf( pvar->left, sum - pvar->val, ans, curr );
    rootleaf( pvar->right, sum - pvar->val, ans, curr );
    if( pvar->left == NULL && pvar->right == NULL && ( (sum - pvar->val ) == 0 ) )
        ans.push_back( curr );
}

vector<vector <int>> Solution::pathSum(TreeNode* A, int B) {
    vector< vector<int> > ans;
    vector<int> curr;
    rootleaf( A, B, ans, curr );
    return ans;
}
```
