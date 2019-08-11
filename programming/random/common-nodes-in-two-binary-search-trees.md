# Common Nodes in Two Binary Search Trees

https://www.interviewbit.com/problems/common-nodes-in-two-binary-search-trees/?ref=random-problem/

Given two BSTs, return the sum of all common nodes in both.
In case there is no common node, return 0

### NOTE

1. Your code will run on multiple test cases, please come up with an optimised solution.
2. Try to do it one pass through the trees.

### INPUT FORMAT
```
A : Root of Tree A
B : Root of Tree B
```

### EXAMPLE INPUT

```
Tree A:
    5
   / \
  2   8
   \   \
    3   15
        /
        7

Tree B:
    7
   / \
  1  10
   \   \
    2  15
       /
      8
```
### EXAMPLE OUTPUT
```
38
```
### EXAMPLE EXPLANATION
```
Common Nodes are : 2, 7, 8, 15
So answer is 2 + 7 + 8 + 15 = 32
```

## Solution Approach

### Simple Solution

A simple way is to one by once search every node of first tree in second tree. Time complexity of this solution is O(m * h) where m is number of nodes in first tree and h is height of second tree.

### Linear Time

We can find common elements in O(n) time.
a. Do inorder traversal of first tree and store the traversal in an auxiliary array ar1[]. See sortedInorder() here.
b. Do inorder traversal of second tree and store the traversal in an auxiliary array ar2[]
c. Find intersection of ar1[] and ar2[]. See this for details https://www.geeksforgeeks.org/union-and-intersection-of-two-sorted-arrays-2/
   
   Time complexity of this method is O(m+n) where m and n are number of nodes in first and second tree respectively. This solution requires O(m+n) extra space.

### Linear Time and limited Extra Space

We can find common elements in O(n) time and O(h1 + h2) extra space where h1 and h2 are heights of first and second BSTs respectively.
The idea is to use iterative inorder traversal. We use two auxiliary stacks for two BSTs. Since we need to find common elements, whenever we get same element, we print it.


## Solution 
### Editorial
```cpp
set<int> st;
void in_order(TreeNode* root){
    if(!root) return;
    st.insert(root->val);
    in_order(root->left);
    in_order(root->right);
}

int getSum(TreeNode* root){
    if(!root) return 0;
    int ans = 0;
    if(st.count(root->val))
        ans += root->val;
    return ans + getSum(root->left) + getSum(root->right);
}

int Solution::solve(TreeNode* A, TreeNode* B){
    st.clear();
    in_order(A);
    return getSum(B);
}
```
### Fastest
```cpp
TreeNode* fun(TreeNode* root,TreeNode* gift)
{
    if(root==NULL)
        return gift;
    TreeNode * r=fun(root->right,gift);
    root->right=r;
    TreeNode * l=fun(root->left,root);
    return l;
}
int Solution::solve(TreeNode* A, TreeNode* B) {
    
    TreeNode* head1=fun(A,NULL);
    TreeNode* head2=fun(B,NULL);
    
    TreeNode* temp1=head1;
    TreeNode* temp2=head2;
    int sum=0;
    while(temp1!=NULL && temp2!=NULL)
    {
        if(temp1->val==temp2->val)
        {
            sum+=temp1->val;
            temp1=temp1->right;
            temp2=temp2->right;
        }
        else if(temp1->val<temp2->val)
        {
            temp1=temp1->right;
        }
        else
        {
            temp2=temp2->right;
        }
    }
    return sum;
}
```
### Lightweight
```cpp
bool check(int x, TreeNode* B)
 {
     if(!B) return false;
     if(x == B->val)return true;
     else if(x > B->val) return check(x,B->right);
     else return check(x, B->left);
   //  return false;
 }
int Solution::solve(TreeNode* A, TreeNode* B) {
    int x;
    if(!A) return 0;
    if(check(A->val, B))
        x = A->val;
    return x + solve(A->left, B) + solve(A->right, B);
    
}
```
### Mine
```cpp
void dfs(TreeNode *root, unordered_map<int, int>& m) {
    if (!root) return;
    m[root->val]++;
    dfs(root->left, m);
    dfs(root->right, m);
}
 
int Solution::solve(TreeNode* A, TreeNode* B) {
    unordered_map<int, int> m;
    dfs(A, m);
    dfs(B, m);
    int res = 0;
    for (auto &p:m)
        if (p.second>1)
            res += p.first;
    return res;
}
```
