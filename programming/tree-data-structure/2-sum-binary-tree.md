# 2sum Binary Tree

https://www.interviewbit.com/problems/2sum-binary-tree





How would you solve with O(N) memory?
Let's say you are given a sorted array and you need to find two indices i < j,
such that A[i] = A[j]. Can you relate between a sorted array and a BST?
Can you avoid O(N) memory and do with O(height) memory?


How would you solve with O(N) memory? Let's say you are given a sorted array and you need to find
two indices i < j, such that A[i] = A[j]. Can you relate between a sorted array and a BST?
Can you avoid O(N) memory and do with O(height) memory?

If you do inorder traversal of BST you visit elements in increasing order. So, we use a two pointer approach,
where we keep two pointers pt1 and pt2. Initially pt1 is at smallest value and pt2 at largest value.

Then we compare sum = pt1.value + pt2.value. If sum < target, we increase pt2, else we decrease pt2 until we reach target.

Using the same concept used in https://www.interviewbit.com/courses/programming/topics/trees/problems/treeiterator/
we can do this.

## Solution

```cpp

// fastest

void recurse(TreeNode* cur, vector<int>& num) {
   if(cur == NULL) {
       return;
   }
   recurse(cur->left, num);
   num.push_back(cur->val);
   recurse(cur->right, num);
   delete cur;
}

int Solution::t2Sum(TreeNode* A, int B) {
    vector<int> num;
    recurse(A, num);
    int low=0, high=num.size()-1;
    while(low < high) {
        if(num[low]+num[high] > B) {
            high--;
        } else if(num[low] + num[high] < B) {
            low++;
        } else {
            return 1;
        }
    }
    return 0;
}


//////////////

void helper(TreeNode *A, vector<int> &B) {
    if (A->left) helper(A->left, B);
    B.push_back(A->val);
    if (A->right) helper(A->right, B);
    return;
}

int Solution::t2Sum(TreeNode *A, int B) {
    vector<int> Inorder;
    helper(A, Inorder);
    int n = Inorder.size();
    for (int i = 0, j = n - 1; i < j;) {
        if (Inorder[i] + Inorder[j] == B) return 1;
        if (Inorder[i] + Inorder[j] < B)
            i++;
        else if (Inorder[i] + Inorder[j] > B)
            j--;
    }
    return 0;
}
```