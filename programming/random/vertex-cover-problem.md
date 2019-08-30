# Vertex Cover Problem

https://www.interviewbit.com/problems/vertex-cover-problem


A vertex cover of an undirected graph is a subset of its vertices such that for every edge (u, v) of the graph, either ‘u’ or ‘v’ is in vertex cover.

Given a binary tree with N nodes, find and return the minimum of nodes in its vertex cover.

### Input Format:

The first and the only argument has a pointer to the root node of the binary tree.

### Output Format

Return an integer, representing the answer as described in the question.

### Constraints

1 <= N <= 1e5

### For Example
```
Input 1:
            1
           / \
          15  2

Output 1:
    1

Explanation 1:
    [1] is enough to be the vertex cover of the tree.

Input 2:
            1
        
Output 2:
        0

Explanation 2:
    [] is enough to be the vertex cover of the tree.

Input 3:
            9
           / \
          6  17
         / \
        23  7
        
Output 3:
        2

Explanation 3:
    [9, 6] is enough to be the vertex cover of the tree.
```

## Solution
### Editorial
```cpp
void temp(TreeNode *root){
    if(root == NULL)
        return;
    root->val = 0;
    temp(root->left);
    temp(root->right);
}

int vCover(TreeNode *root) 
{
    if (root == NULL) 
        return 0; 
    if (root->left == NULL && root->right == NULL) 
        return 0; 
    
    if (root->val != 0) 
        return root->val; 
  
    int size_incl = 1 + vCover(root->left) + vCover(root->right); 
  
    int size_excl = 0; 
    if (root->left) 
      size_excl += 1 + vCover(root->left->left) + vCover(root->left->right); 
    if (root->right) 
      size_excl += 1 + vCover(root->right->left) + vCover(root->right->right); 
  
    root->val =  min(size_incl, size_excl); 
  
    return root->val; 
} 
 
int Solution::solve(TreeNode* A) {
    temp(A);
    return vCover(A);
}

```

### Fastest
```cpp
void trav(TreeNode *A)
 {
    if(A==NULL)
        return ;
    A->val=0;
    trav(A->left);
    trav(A->right);
 }
 int v(TreeNode* A)
 {
     if(A==NULL)
        return 0;
    if(A->right==NULL && A->left==NULL)
        return 0;
    if(A->val!=0)
        return A->val;
    int s1=1+v(A->left)+v(A->right);
    int s2=0;
    if(A->left!=NULL)
        s2+=1+v(A->left->left)+v(A->left->right);
    if(A->right!=NULL)
        s2+=1+v(A->right->left)+v(A->right->right);
    return A->val=min(s2,s1);    
 }
int Solution::solve(TreeNode* A) {
    int count=0;
    trav(A);
    return v(A);
}
```

### Lightweight
```cpp
void trav(TreeNode *A)
 {
    if(A==NULL)
        return ;
    A->val=0;
    trav(A->left);
    trav(A->right);
 }
 int v(TreeNode* A)
 {
     if(A==NULL)
        return 0;
    if(A->right==NULL && A->left==NULL)
        return 0;
    if(A->val!=0)
        return A->val;
    int s1=1+v(A->left)+v(A->right);
    int s2=0;
    if(A->left!=NULL)
        s2+=1+v(A->left->left)+v(A->left->right);
    if(A->right!=NULL)
        s2+=1+v(A->right->left)+v(A->right->right);
    return A->val=min(s2,s1);    
 }
int Solution::solve(TreeNode* A) {
    int count=0;
    trav(A);
    return v(A);
}
```

### Mine
```cpp
int vCover(TreeNode *root) {
    if (root == NULL) 
        return 0; 
    if (root->left == NULL && root->right == NULL) 
        return 0;
    if (root->val != 0) 
        return root->val; 
    int size_incl = 1 + vCover(root->left) + vCover(root->right); 
    int size_excl = 0; 
    if (root->left) 
      size_excl += 1 + vCover(root->left->left) + vCover(root->left->right); 
    if (root->right) 
      size_excl += 1 + vCover(root->right->left) + vCover(root->right->right);
    root->val =  min(size_incl, size_excl); 
    return root->val; 
}

void set_val(TreeNode *root, int val) {
    if (!root) return;
    root->val = val;
    set_val(root->left, val);
    set_val(root->right, val);
}
    
int Solution::solve(TreeNode* A) {
    set_val(A, 0);
    return vCover(A);
}
```
