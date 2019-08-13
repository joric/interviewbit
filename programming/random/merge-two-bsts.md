# Merge Two BSTs

https://www.interviewbit.com/problems/merge-two-bsts

Given two binary search tree A and B of size N and M.
Return an array of integers containing elements of both BST in non-decreasing order.

### Note

Expected time complexity O(M+N)

Expected Auxillary Space (H(A) + H(B)) where H(A) is height of tree A and H(B) is height of tree B.

### Constraints
```
1 <= N, M <= 100000
```
### For Example
```
Input 1:
            5            7
           / \          / \
          3   8        2   9
     
Output 1:
    [2, 3, 5, 7, 8, 9]

Input 1:
            1            5
             \          / \
              4        2  10
     
Output 1:
    [1, 2, 4, 5, 10]
```

## Solution Approach

The idea is to use iterative inorder traversal.
We use two auxiliary stacks for two BSTs. Let say stack s1 for A and stack s2 for B.
Since we need to return the elements in sorted form,
whenever we get a smaller element from any of the trees, we append it to our answer array. 
If the element is greater, then we push it back to stack for the next iteration.

## Solution
### Editorial
```cpp
class NextIter{
private:
    stack<TreeNode*> st;
public:
    
    void push(TreeNode* root){
        TreeNode* curr = root;
        while(curr){
            st.push(curr);
            curr = curr->left;
        }
    }

    void init(TreeNode* root){
        push(root);
    }
    
    bool hasNext(){
        return !st.empty();
    }
    
    TreeNode* nextTop(){
        if(hasNext()){
            auto curr = st.top();
            return curr;
        }
        return NULL;
    }
    
    TreeNode* nextPop(){
        if(hasNext()){
            auto curr = st.top(); st.pop();
            if(curr->right){
                push(curr->right);
            }
            return curr;
        }
        return NULL;
    }
};


vector<int> Solution::solve(TreeNode* A, TreeNode* B){
    NextIter it1, it2;
    it1.init(A);
    it2.init(B);
    vector<int> res;
    while(it1.hasNext() && it2.hasNext()){
        if(it1.nextTop()->val < it2.nextTop()->val){
            res.push_back(it1.nextTop()->val); it1.nextPop();
        }
        else{
            res.push_back(it2.nextTop()->val); it2.nextPop();
        }
    }
    
    while(it1.hasNext()){
        res.push_back(it1.nextTop()->val); it1.nextPop();
    }
    
    while(it2.hasNext()){
        res.push_back(it2.nextTop()->val); it2.nextPop();
    }
    return res;
}
```

### Fastest
```cpp
vector<int> Solution::solve(TreeNode* A, TreeNode* B) {
     vector<int>ans;
    stack<TreeNode*>stack1;
    stack<TreeNode*>stack2;
    TreeNode* temp1=A;
    TreeNode* temp2=B;
    while(!stack1.empty() || !stack2.empty() || temp1 || temp2)
    {
        while (temp1)
        {
            stack1.push(temp1);
            temp1=temp1->left;
        }
        while (temp2)
        {
            stack2.push(temp2);
            temp2=temp2->left;
        }
        if(stack2.empty() || (!stack1.empty() && stack1.top()->val<=stack2.top()->val))
            {
                temp1=stack1.top();
                stack1.pop();
                ans.push_back(temp1->val);
                temp1=temp1->right;
            }
       else if(stack1.empty() || (stack2.empty() && stack1.top()->val>stack2.top()->val))
            {
                temp2=stack2.top();
                stack2.pop();
                ans.push_back(temp2->val);
                temp2=temp2->right;
            }
    }

    return ans;
}
```

### Lightweight
```cpp
vector<int> Solution::solve(TreeNode* A, TreeNode* B) {
    
    stack<TreeNode * > a;
    stack<TreeNode * > b;
    
    vector<int> ans;
    
    TreeNode * ta = A;
    TreeNode * tb = B;
    while(ta!=NULL){
        a.push(ta);
        ta = ta->left;
    }
    while(tb!=NULL){
        b.push(tb);
        tb = tb->left;
    }
    
    while(!a.empty() && !b.empty()){
        
        if(a.top()->val<=b.top()->val){
            ans.push_back(a.top()->val);
            ta = a.top()->right;
            a.pop();
            while(ta!=NULL){
                a.push(ta);
                ta = ta->left;
            }
            
        }
        else{
            ans.push_back(b.top()->val);
            ta = b.top()->right;
            b.pop();
            while(ta!=NULL){
                b.push(ta);
                ta = ta->left;
            }
        }
        
    }
    
    while(!a.empty()){
        ans.push_back(a.top()->val);
            ta = a.top()->right;
            a.pop();
            while(ta!=NULL){
                a.push(ta);
                ta = ta->left;
            }
    }
    
    while(!b.empty()){
        ans.push_back(b.top()->val);
            ta = b.top()->right;
            b.pop();
            while(ta!=NULL){
                b.push(ta);
                ta = ta->left;
            }
    }
    
    
    return ans;
}
```

### Mine
```cpp
void inorder(TreeNode *root, vector<int> &res) {
    if (!root) return;
    inorder(root->left, res);
    res.push_back(root->val);
    inorder(root->right, res);
}

void merge(TreeNode *root1, TreeNode *root2, vector<int> &res) {
    stack<TreeNode *> s1;
    stack<TreeNode *> s2;
    TreeNode *c1 = root1, *c2 = root2;

    while (c1 || !s1.empty() || c2 || !s2.empty()) {
        if (c1 || c2 ) {
            if (c1) {
                s1.push(c1);
                c1 = c1->left;
            }
            if (c2) {
                s2.push(c2);
                c2 = c2->left;
            }

        } else {
            if (s1.empty()) {
                while (!s2.empty()) {
                    c2 = s2.top();
                    s2.pop();
                    c2->left = 0;
                    inorder(c2, res);
                }
                return;
            }
            if (s2.empty()) {
                while (!s1.empty()) {
                    c1 = s1.top();
                    s1.pop();
                    c1->left = 0;
                    inorder(c1, res);
                }
                return;
            }

            c1 = s1.top(); s1.pop();
            c2 = s2.top(); s2.pop();

            if (c1->val < c2->val) {
                res.push_back(c1->val);
                c1 = c1->right;
                s2.push(c2);
                c2 = 0;
            } else {
                res.push_back(c2->val);
                c2 = c2->right;
                s1.push(c1);
                c1 = 0;
            }
        }
    }
}

vector<int> Solution::solve(TreeNode *A, TreeNode *B) {
    vector<int> res;
    merge(A, B, res);
    return res;
}
```
