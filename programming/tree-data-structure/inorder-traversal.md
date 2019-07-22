# Inorder Traversal

https://www.interviewbit.com/problems/inorder-traversal


You can do this problem easily but as stated in problem recursion is not allowed here.

Stack can help you to avoid recursion. How?

Think stack.

Recursive call would look something like this :

inorderprint(root->left);
print(root->val);
inorderprint(root->right);

Instead of calling the functions, can you put the nodes on a stack and process them ?

How would your solution work if you were allowed to change the original tree ? 
How would it work if you were not allowed to change the tree but use additional memory ( track the number of times a node has appeared in the tree ) ? 
How would it work if you were not even allowed the extra memory ?

## Solution

```cpp
// editorial

vector<int> inorderTraversal(TreeNode *root) {
    vector<int> vector;
    stack<TreeNode *> stack;
    TreeNode *pCurrent = root;

    while (!stack.empty() || pCurrent) {
        if (pCurrent) {
            stack.push(pCurrent);
            pCurrent = pCurrent->left;
        } else {
            TreeNode *pNode = stack.top();
            vector.push_back(pNode->val);
            stack.pop();
            pCurrent = pNode->right;
        }
    }
    return vector;
}
```