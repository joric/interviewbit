# Preorder Traversal

https://www.interviewbit.com/problems/preorder-traversal


## Hint 1
You can do this problem easily but as stated in problem recursion is not allowed here.

Stack can help you to avoid recursion. How?


Think stack.

Recursive call would look something like this :

print(root->val);
preorderprint(root->left);
preorderprint(root->right);

Instead of calling the functions, can you put the nodes on a stack and process them ?



## Solution

```cpp

// editorial

vector<int> Solution::preorderTraversal(TreeNode *root) {
    vector<int> res;
    stack<TreeNode *> nodeStack;

    while (nodeStack.size() != 0 || root != NULL) {
        if (root == NULL) {
            root = nodeStack.top();
            nodeStack.pop();
        } else {
            res.push_back(root->val);
            if (root->right != NULL)
                nodeStack.push(root->right);
            root = root->left;
        }
    }
    return res;
}
```