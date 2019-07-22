# Postorder Traversal

https://www.interviewbit.com/problems/postorder-traversal


You can do this problem easily but as stated in problem recursion is not allowed here.

Stack can help you to avoid recursion. How?


Think stack.

Recursive call would look something like this :

postorderprint(root->left);
postorderprint(root->right);
print(root->val);

Instead of calling the functions, can you put the nodes on a stack and process them ? 
Would the solution be easier if you were to print the reverse of the asked ?



pre-order traversal is root-left-right, and post order is left-right-root. modify the code for pre-order to make it root-right-left, and then reverse the output so that we can get left-right-root .

Create an empty stack, Push root node to the stack.
Do following while stack is not empty.

2.1. pop an item from the stack and print it.

2.2. push the left child of popped item to stack.

2.3. push the right child of popped item to stack.

reverse the ouput.

## Solution

```cpp

// editorial

vector<int> postorderTraversal(TreeNode *root) {
    stack<TreeNode *> nodeStack;
    vector<int> result;
    //base case
    if (root == NULL)
        return result;
    nodeStack.push(root);
    while (!nodeStack.empty()) {
        TreeNode *node = nodeStack.top();
        result.push_back(node->val);
        nodeStack.pop();
        if (node->left)
            nodeStack.push(node->left);
        if (node->right)
            nodeStack.push(node->right);
    }
    reverse(result.begin(), result.end());
    return result;
}
```