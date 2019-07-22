# Next Pointer Binary Tree

https://www.interviewbit.com/problems/next-pointer-binary-tree/

Given a binary tree
```
    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

 Note:
You may only use constant extra space.
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).
Example :

Given the following perfect binary tree,
```
         1
       /  \
      2    5
     / \  / \
    3  4  6  7
```
After calling your function, the tree should look like:
```
         1 -> NULL
       /  \
      2 -> 5 -> NULL
     / \  / \
    3->4->6->7 -> NULL
```
Note that using recursion has memory overhead and does not qualify for constant space.

## Solution

### Editorial
```cpp
void levelOrderTraversal(TreeLinkNode *root, unordered_map<int, TreeLinkNode *> &ret, int depth) {
    if (!root)
        return;
    if (ret[depth])
        ret[depth]->next = root;
    ret[depth] = root;
    levelOrderTraversal(root->left, ret, depth + 1);
    levelOrderTraversal(root->right, ret, depth + 1);
}

void Solution::connect(TreeLinkNode *A) {
    unordered_map<int, TreeLinkNode *> ret;
    levelOrderTraversal(A, ret, 0);
}

```

### Mine

```cpp
void Solution::connect(TreeLinkNode *root) {
    for (TreeLinkNode *i = root; i->left; i = i->left) {
        for (TreeLinkNode *j = i; j; j = j->next) {
            j->left->next = j->right;
            if (j->next)
                j->right->next = j->next->left;
        }
    }
}

```
