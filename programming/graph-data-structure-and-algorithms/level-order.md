# Level Order
https://www.interviewbit.com/problems/level-order/

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

### Example

Given binary tree
```
    3
   / \
  9  20
    /  \
   15   7
```
return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
]
```

Also think about a version of the question where you are asked to do a level order traversal of the tree when depth of the tree is much greater than number of nodes on a level.


## Hint 1

Can you maintain depth of a node while traversing the tree. How can it help you after the tree traversal?

## Solution Approach

There are 2 ways to solve this problem.

Approach 1: Maintain a vector of size 'depth' of the tree. Do any kind of tree traversal keeping track of the current depth. Append the current element to vector[currentDepth]. Since we need stuff left to right, make sure left subtree is visited before the right subtree ( Any of traditional pre/post/inorder traversal should suffice ).

Approach 2: This is important. A lot of times, you'd be asked to do a traditional level order traversal. Or to put in formal words, a traversal where the extra memory used should be proportional to the nodes on a level rather than the depth of the tree. To do that, you need to make sure you are accessing all the nodes on a level before accessing the nodes on next. This is a typical breadth first search problem. Queue FTW.

## Solution

### Editorial

```cpp
void buildVector(TreeNode *root, int depth, vector<vector<int> > &ret) {
    if(root == NULL) return;
    if(ret.size() == depth)
        ret.push_back(vector<int>());
    ret[depth].push_back(root->val);
    buildVector(root->left, depth + 1, ret);
    buildVector(root->right, depth + 1, ret);
}

vector<vector<int> > Solution::levelOrder(TreeNode *root) {
    vector<vector<int> > ret;
    buildVector(root, 0, ret);
    return ret;
}
```
### Lightweight

```cpp
vector<vector<int>> Solution::levelOrder(TreeNode *A) {
    int levelSize = 1, nextLevel = 0;
    vector<vector<int>> result;
    vector<int> curLevel;

    if (A == NULL)
        return result;

    queue<TreeNode *> q;

    q.push(A);

    while (q.empty() != true) {
        TreeNode *node = q.front();
        q.pop();

        if (node->left != NULL) {
            q.push(node->left);
            nextLevel++;
        }

        if (node->right != NULL) {
            q.push(node->right);
            nextLevel++;
        }

        levelSize--;
        curLevel.push_back(node->val);
        if (levelSize == 0) {
            result.push_back(curLevel);
            levelSize = nextLevel;
            nextLevel = 0;
            curLevel.clear();
        }
        delete (node);
    }

    return result;
}

```

## Asked in
* Facebook
* Groupon
* Goldman Sachs

