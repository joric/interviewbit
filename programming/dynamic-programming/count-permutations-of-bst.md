# Count Permutations of BST

https://www.interviewbit.com/problems/count-permutations-of-bst/


You are given two positive integers A and B. For all permutations of [1, 2, ..., A], we create a BST. Count how many of these have height B.

Notes:

1. Values of a permutation are sequentially inserted into the BST by general rules i.e in increasing order of indices.
2. Height of BST is maximum number of edges between root and a leaf.
3. Return answer modulo 109 + 7.
4. Expected time complexity is worst case O(N4).
5. 1 <= N <= 50

For example,

A = 3, B = 1

Two permutations [2, 1, 3] and [2, 3, 1] generate a BST of height 1.
In both cases the BST formed is

```
    2
   / \
  1   3
```

Another example,

A = 3, B = 2

Return 4.

Next question, can you do the problem in O(N3)?

## Hint 1

BST follows the property that all values in left subtree and less than value at current node and all values in right subtree are greater than current node.
If we fix the root node, the BST formed will be unique.

Also, the actual values that are being inserted in BST don't matter. So, we can directly deal with number of values being inserted in BST instead of the actual values. This helps in defining states of DP.

Now, what should be the states of DP? Of course, number of elements is one state. Other can be the height required.

## Solution Approach

BST follows the property that all values in left subtree and less than value at current node and all values in right subtree are greater than current node.
If we fix the root node, the BST formed will be unique.

Also, the actual values that are being inserted in BST don't matter. So, we can directly deal with number of values being inserted in BST instead of the actual values. This helps in defining states of DP.

Now, what should be the states of DP? Of course, number of elements is one state. Other can be the height required.

So, we define DP(N, M) as the number of permutations of N elements which when inserted into BSTs generate BSTs of height exactly M.

Now, to define a recurrence, we'll iterate over the root of BST we choose. We have N options and based on each option, the size of left and right subtrees are defined.

If i'th element is choosen as root, the left subtree will now contain (i - 1) elements and right subtree will contain (N - i) elements.

Now, at least one of these subtrees must have a height of (M - 1) because we are right now solving for height M.

Again, we'll iterate over the heights of left and right subtrees.

Now, number of permutations to form left subtree of size x with some height are say, X. Also, we call these permutations set A.

And similarly, number of permutations to form right subtree of size y with some height are say, Y. And we call these permutations set B.

Now, we can choose any permutation from A and any permutation from B, to form a unique tree. So, there are total of X*Y permutations. Also, any sequence of size (x+y) can give the same BST if the mutual ordering of the permutation from set A and permutation of set B is maintained. There are choose(x + y, y) ways to do that. So, total ways are X*Y*choose(x + y, y).

So, in terms of pseudo code:
```
def rec(N, M):
    if N<=1:
        if M==0: return 1
        return 0;

    ret=0

    for i=1 to N:
        x = i-1
        y = N-i

        ret1=0

        //iterate over height of left subtree
        for j = 0 to M-2:
            ret1 = ret1 + rec(x, j)*rec(y, M-1)

        //iterate over height of right subtree
        for j = 0 to M-2:
            ret1 = ret1 + rec(y, j)*rec(x, M-1)

        //add the case when both heights=M-1
        ret1 = ret1 + rec(x, M-1)*rec(y, M-1)

        ret = ret + ret1*choose(x+y, y)

    return ret
```

We can precalculate choose table in O(N*N).

Also, take care of modulo arithmetic.

## Solution

### Editorial
```cpp
#define MOD 1000000007ll
typedef long long LL;

//adds y to x, modulo MOD
void add(int &x, int y){
    x += y;
    if(x>=MOD)x-=MOD;
}

//choose and dp tables
vector< vector<int > > choose,dp;

//build choose table
void build(int N){
    choose[0][0]=1;
    for(int i=1; i<=2*N; i++){
        choose[i][0]=1;
        for(int j=1; j<=i; j++){
            choose[i][j]=choose[i-1][j];
            add(choose[i][j], choose[i-1][j-1]);
        }
    }
}

//reccurence function as defined in hint_2
int rec(int n, int h){ 
    if(n<=1)return (h==0);
    if(h<0)return 0;
    int &ret=dp[n][h];
    if(ret!=-1)return ret;
    ret=0;
    int x, y;
    for(int i=1; i<=n; i++){
        x=i-1;
        y=n-x-1;
        int sum1=0,sum2=0,ret1=0;
        for(int j=0; j<=h-2; j++){
            add(sum1, rec(x, j));
            add(sum2, rec(y, j));
        }
        add(ret1, ((LL)sum1*(LL)rec(y, h-1))%MOD);
        add(ret1, ((LL)sum2*(LL)rec(x, h-1))%MOD);
        add(ret1, ((LL)rec(x, h-1)*(LL)rec(y, h-1))%MOD);
        ret1 = ((LL)ret1*(LL)choose[x+y][y])%MOD;
        add(ret, ret1);
    }
    return ret;
}

int Solution::cntPermBST(int A, int B) {
    int n=50;
    choose.clear();
    dp.clear();
    choose.resize(2*n+1,vector<int>(2*n+1, 0));
    dp.resize(n+1,vector<int>(n+1, -1));
    build(n);
    return rec(A, B);
}

```
