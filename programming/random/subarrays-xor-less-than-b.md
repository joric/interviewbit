# Subarrays Xor less Than B

https://www.interviewbit.com/problems/subarrays-xor-less-than-b/

Given an array of integers A.Find and return the number of subarrays whose xor values is less than B.

Since the number of subarrays can be very large you are required to
return the number of subarrays whose xor values is less than B modulo (109+7).

### Input Format

The argument given is the integer array A and an integer B.

### Output Format

return the number of subarrays whose xor values is less than B modulo (10^9+7).

### Constraints
```
1 <= length of the array <= 100000
1 <= A[i] <= 10^5
1 <= B <= 10^6
```

### For Example
```
Input 1:
    A = [8, 3, 10, 2, 6, 7, 6, 9, 3]
    B = 3
Output 1:
    5

Input 2:
    A = [9, 4, 3, 11]
    B = 7
Output 2:
    3
```

## Solution
### Editorial
```cpp
const long long mod=1000000007;

struct node {
    node* left;
    node* right;
    node* parent;
    int leaf = 1;
    node(){
    left=NULL;
    right=NULL;
    parent=NULL;
    leaf=1;
    }
};

void update(node* root) {
    if (root->right && root->left)
        root->leaf = root->right->leaf + root->left->leaf;
    else if (root->left)
        root->leaf = root->left->leaf;
    else if (root->right)
        root->leaf = root->right->leaf;
    if (root->parent)
        update(root->parent);
}

void insert(string num, int level, node* root) {
    if (level == -1) {
        update(root);
        return;
    }
    int x = num[level] - '0';
    if (x == 1) {
        if (!root->right) {
            struct node* temp = new node();
            root->right = temp;
            temp->parent = root;
        }
        insert(num, level - 1, root->right);
    }
    else{

        if (!root->left) {
            struct node* temp = new node;
            temp=new node();
            root->left = temp;
            temp->parent = root;
        }
        insert(num, level - 1, root->left);
    }
}

void solveUtil(string num, string k, int level, node* root, long long& ans) {
    if (level == -1)
        return;
    if (num[level] == '1') {
        if (k[level] == '1') {
            if (root->right)
                ans += 1LL*(root->right->leaf);
            if (root->left)
                solveUtil(num, k, level - 1, root->left, ans);
        }
        else{
            if (root->right)
                solveUtil(num, k, level - 1, root->right, ans);
        }

    }
    else{
        if (k[level] == '0') {
            if (root->left)
                solveUtil(num, k, level - 1, root->left, ans);
        }
        else  {
            if (root->left)
                ans += 1LL*(root->left->leaf);
            if (root->right)
                solveUtil(num, k, level - 1, root->right, ans);
        }
    }
}


int doit(vector<int> &a, int K) {
    int n = a.size();
    int maxEle = K;
    node *head =new node();
    for (int i = 0; i < n; i++)
        maxEle = max(maxEle, a[i]);
    int height = (int)ceil(1.0 * log2(maxEle)) + 1;
    string k = "";
    int temp = K;
    for (int j = 0; j < height; j++) {
        k = k + char(temp % 2 + '0');
        temp /= 2;
    }

    string init = "";
    for (int i = 0; i < height; i++)
        init += '0';
    insert(init, height - 1, head);
    long long ans = 0;
    temp = 0;
    for (int i = 0; i < n; i++) {
        string s = "";
        temp = (temp ^ a[i]);
        for (int j = 0; j < height; j++) {
            s = s + char(temp % 2 + '0');
            temp = temp >> 1;
        }
        solveUtil(s, k, height - 1, head, ans);

        insert(s, height - 1, head);
    }
    return ans%mod;
}

int Solution::solve(vector<int> &A, int B) {
  return doit(A,B);
}
```

### Fastest
```cpp
struct Node{
    int one, zero;
    Node *left, *right;
    Node(){
        one = zero = 0;
        left = NULL;
        right = NULL;
    }
};

void add(Node *root, int num ){
    for( int i = 20; i >= 0; i-- ){
        int bit = (num>>i)&1;
        if( bit ){
            if( root->right == NULL )
                root->right = new Node();
            
            root->one++;
            root = root->right;
        }
        else{
            if( root->left == NULL )
                root->left = new Node();
            root->zero++;
            root = root->left;
        }
    }
}

int query( Node *root, int num, int k ){
    int sum = 0;
    for( int i = 20; i >= 0; i-- ){
        if( root == NULL )return sum;
        int have = (num>>i)&1;
        int req = (k>>i)&1;
        //cout << "(" << req << "," << have << ") ";
        if( req == 1){
            if( have == 1 ){ // 1 all 1 add
                sum += root->one;
                root = root->left;
            }
            else{ // 0 all 0 add 
                sum += root->zero;
                root = root->right;
            }
        }
        else{
            if( have == 1 ) root = root->right;
            else root = root->left;
        }
    }
    return sum;
}

int Solution::solve(vector<int> &A, int B) {
    int pre = 0;
    Node *root = new Node();  
    add(root, 0);
    long long sum = 0;
    for( auto num : A ){
        pre ^= num;
        sum += query(root, pre, B);
        add(root, pre);
        sum %= 1000000007;
    }
    return sum;
}
```

