# Ways To Form Max Heap

https://www.interviewbit.com/problems/ways-to-form-max-heap

Max Heap is a special kind of complete binary tree in which for every node the value present in that node is greater than the value present in it's children nodes. If you want to know more about Heaps, please visit this link

So now the problem statement for this question is:

How many distinct Max Heap can be made from n distinct integers

In short, you have to ensure the following properties for the max heap :

Heap has to be a complete binary tree ( A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible. )
Every node is greater than all its children
Let us take an example of 4 distinct integers. Without loss of generality let us take 1 2 3 4 as our 4 distinct integers

Following are the possible max heaps from these 4 numbers :
```
         4 
       /  \ 
      3   2 
     / 
    1
         4 
       /  \ 
      2   3 
     / 
    1
        4 
       /  \ 
      3   1 
     / 
    2
```
These are the only possible 3 distinct max heaps possible for 4 distinct elements.

Implement the below function to return the number of distinct Max Heaps that is possible from n distinct elements.

As the final answer can be very large output your answer modulo 1000000007

Input Constraints: n <= 100

### Note

Note that even though constraints are mentioned for this problem, for most problems on InterviewBit, constraints are intentionally left out. In real interviews, the interviewer might not disclose constraints and ask you to do the best you can do. 

## Hint 1

Verify if the following is true :

1. The structure of the heap tree will be fixed.
2. Given the numbers in a subtree, the root value is going to be fixed.

Are the numbers in left and right subtree independent of each other?

## Solution Approach

Suppose there are n distinct elements to be used in Max heap. Now it is for sure that the maximum element among this n distinct element will surely be placed on the root of the heap.

1. Now there will be remaining (n-1) elements to be arranged.
2. Now point to be remembered here is that the structure of the heap will remain the same. That is only the values within the node will change however the overall structure remaining the same.
3. As structure of the heap remains the same, the number of elements that are present in the left sub-tree of the root (L) will be fixed. And similarly the number of the elements on the right sub-tree (R) of the root. And also following equality holds .

`L + R = (n-1)`

1. Now as all the remaining (n-1) elements are less than the element present at the root(The Max Element), whichever L elements among 'n-1` elements we put in the left sub-tree, it doesn't matter because it will satisfy the Max Heap property.
2. So now there are (n-1)/C/L ways to pickup L elements from (n-1) elements. And then recurse the solution.
3. So final equation will be as follows :

`(n-1)/C/L * getNumberOfMaxHeaps(L) * getNumberOfMaxHeaps(R)`

1. So now the question remains only of finding L for given n. It can be found as follows:
2. Find the height of the heap h = log2(n)
3. Find the max number of elements that can be present in the hth level of any heap . Lets call it m. m = 2^h
4. Find the number of elements that are actually present in last level(h^th level) in heap of size n. Lets call it p. p = n - (2^h - 1)
5. Now if the last level of the heap is more than or equal to exactly half filled, then L would be 2^h - 1
6. However if it is half filled then it will reduced by the number of elements in last level left to fill exactly half of the last level.
7. So final equation for L will be as follows :

```
L = 2^h - 1 if p >= m/2
L = 2^h - 1 - (m/2 - p) if p < m/2
```

## Solution

### Editorial
```cpp
long long int combination(int n, int r, int mod) {
    int C[r + 1];
    memset(C, 0, sizeof(C));

    C[0] = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = min(i, r); j > 0; j--)
            C[j] = (C[j] + C[j - 1]) % mod;
    }
    return C[r];
}

int Solution::solve(int A) {
    int n = A;
    if (A <= 1) {
        return 1;
    } else {
        int h = log2(n);
        int m = pow(2, h);
        int p = n + 1 - m;
        int l;
        if (p >= m / 2)
            l = m - 1;
        else
            l = m / 2 - 1 + p;
        int r = n - 1 - l;
        long long int x = combination(n - 1, l, 1000000007);
        long long int y = solve(l);
        long long int z = solve(r);
        return (((x * y) % 1000000007) * z) % 1000000007;
    }
}
```

### Mine
```cpp
long long dp[105];
long long comb[105][105];
#define MOD 1000000007
int rec(int n) {
    if (n <= 1)
        return 1;
    if (dp[n] != -1)
        return dp[n];
    int i;
    int fill = 0;
    int pw = 2;
    int left, right;
    left = right = 0;

    while (fill + pw < n - 1) {
        fill += pw;
        left += pw / 2;
        right += pw / 2;
        pw *= 2;
    }
    int rem = n - 1 - fill;
    if (rem > pw / 2) {
        left += pw / 2;
        right += (rem - pw / 2);
    } else
        left += rem;

    return dp[n] = (((rec(left) * 1LL * rec(right)) % MOD) * 1LL * comb[n - 1][left]) % MOD;
}
int Solution::solve(int A) {
    int i, j;
    for (i = 0; i <= 100; i++)
        dp[i] = -1;
    comb[0][0] = 1;
    for (i = 1; i <= 100; i++) {
        comb[i][0] = comb[i][i] = 1;
        for (j = 1; j < i; j++)
            comb[i][j] = (comb[i - 1][j - 1] + comb[i - 1][j]) % MOD;
    }
    return rec(A);
}
```
