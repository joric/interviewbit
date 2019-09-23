# Top View

https://www.interviewbit.com/problems/top-view/


Given a binary tree of integers. Return an array of integers representing the Top view of the Binary tree.

Top view of a Binary Tree is a set of nodes visible when the tree is visited from Top side.

Note: Return the nodes in any order.

### Constraints
```
1 <= Number of nodes in binary tree <= 100000
0 <= node values <= 10^9 
```
### For Example

```
Input 1:
            1
          /   \
         2    3
        / \  / \
       4   5 6  7
      /
     8 
Output 1:
    [1, 2, 4, 8, 3, 7]

Input 2:
            1
           /  \
          2    3
           \
            4
             \
              5
Output 2:
    [1, 2, 3]
```

## Solution
### Editorial
```cpp
vector<int> topview(TreeNode* root) 
{ 
    vector <int> ans;
    if(root==NULL) 
       return ans; 
     queue<TreeNode*>q; 
     map<int,int> m;
     map<int,int> l;
     int hd=0; 
     l[root->val] = hd; 
       
     q.push(root); 
      
     while(q.size()) 
     { 
        hd=l[root->val]; 
          
        if(m.count(hd)==0)   
        m[hd]=root->val; 
        if(root->left) 
        { 
            l[root->left->val] = hd-1LL; 
            q.push(root->left); 
        } 
        if(root->right) 
        { 
            l[root->right->val]=hd+1; 
            q.push(root->right); 
        } 
        q.pop(); 
        root=q.front(); 
        
    } 
      
      
     for(auto i=m.begin();i!=m.end();i++) 
    { 
        ans.push_back(i->second); 
    } 
      return ans;
} 

vector<int> Solution::solve(TreeNode* A) {
    return topview(A);
}
```

### Fastest
```cpp
vector<int> Solution::solve(TreeNode* A) 
{
    vector<int> v;
    map<int,int> m;
    queue<pair<TreeNode*,int>> q;
    q.push(make_pair(A,0));
    while(!q.empty())
    {
        pair<TreeNode* , int> temp=q.front();
        q.pop();
        int hd=temp.second;
        TreeNode* n=temp.first;
        if(m.count(hd)==0)
        {
            m[hd]=n->val;
        }
        if(n->left)
        q.push(make_pair(n->left,hd-1));
        if(n->right)
        q.push(make_pair(n->right,hd+1));
    }
    map<int,int>::iterator it;
    for(it=m.begin();it!=m.end();it++)
    {
        v.push_back(it->second);
    }
    return v;
}
```

### Lightweight
```cpp
 void getleft(TreeNode*root, int &lmax, int curr, vector<int> &left)
 {
     if(root!=NULL)
     {
        if(curr>lmax)
        {
            lmax=curr;
            left.push_back(root->val);
        }
        getleft(root->left,lmax,curr+1,left);
        getleft(root->right,lmax,curr-1,left);
     }
 }
 
 void getright(TreeNode*root, int &rmax, int curr, vector<int> &right)
 {
     if(root!=NULL)
     {
        if(curr>rmax)
        {
            rmax = curr;
            right.push_back(root->val);
        }
        getright(root->right,rmax,curr+1,right);
        getright(root->left,rmax,curr-1,right);
     }
 }
 
 
 vector<int> Solution::solve(TreeNode* A) {
    
    
    int lmax = 0; 
    int rmax = 0;
    
    vector<int> left;
    vector<int> right;
    
    getleft(A,lmax,0,left);
    getright(A,rmax,0,right);
    
    vector<int> ans;
    
    for(int i=left.size()-1;i>=0;i--)
    {
        ans.push_back(left[i]);
    }
    
    ans.push_back(A->val);
    
    for(int i=0;i<right.size();i++)
    {
        ans.push_back(right[i]);
    }
    
    return ans;
}
```

### Mine
```cpp
vector<int> Solution::solve(TreeNode* root) {
    vector<int> res;
    if (!root) return res;
    map<TreeNode*, int> hd;
    map<int, int> m;
    deque<TreeNode*> q;
    q.push_back(root);
    hd[root] = 0;
    m[ hd[root] ] = root->val;
    while(!q.empty()) {
        if (root->left) {
            hd[root->left] = hd[root]-1;
            if (!m[hd[root->left]]) m[hd[root->left]] = root->left->val;
            q.push_back(root->left);
        }
        if (root->right) {
            hd[root->right] = hd[root]+1;
            if (!m[hd[root->right]]) m[hd[root->right]] = root->right->val;
            q.push_back(root->right);            
        }
        q.pop_front();
        if (q.size()) root = q.front();
    }
    for (auto &x:m) res.push_back(x.second);
    return res;
}
```
### Another Mine
```cpp
typedef map<int, pair<int, int>> map_t;

void fill_map(TreeNode* root, int d, int l, map_t &m){
    if(!root) return;
    if (!m.count(d) || m[d].second>l) m[d] = make_pair(root->val, l);
    fill_map(root->left, d-1, l+1, m);
    fill_map(root->right, d+1, l+1, m);
}

vector<int> Solution::solve(TreeNode* root) {
    vector<int> res;
    map_t m;
    fill_map(root, 0, 0, m);
    for (auto &x:m) res.push_back(x.second.first);
    return res;
}
```

### Another One
```cpp
vector<int> Solution::solve(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> s;
    TreeNode * curr = root;
    while(curr) {
        s.push(curr);
        curr = curr->left;
    }
    while(!s.empty()) {
        TreeNode * node = s.top();
        s.pop();
        res.push_back(node->val);
    }
    curr = root->right;
    while(curr) {
        res.push_back(curr->val);
        curr = curr->right;
    }
    return res;
}
```
### Another One
```cpp
vector<int> Solution::solve(TreeNode* root) {
    vector<int> res;
    queue<pair<int,TreeNode*>> q;
    q.push(make_pair(0,root));
    map<int, TreeNode*> m;
    while (!q.empty()) {
        auto p = q.front();
        q.pop();
        if (p.second==0) continue;
        m.insert(p);
        q.push(make_pair(p.first+1, p.second->right));
        q.push(make_pair(p.first-1, p.second->left));
    }
    for (auto p:m) res.push_back(p.second->val);
    return res;
}
```

## References
* https://www.geeksforgeeks.org/print-nodes-top-view-binary-tree/
* https://www.hackerrank.com/challenges/tree-top-view/


