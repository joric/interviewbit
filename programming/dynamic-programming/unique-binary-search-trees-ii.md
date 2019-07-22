# Unique Binary Search Trees II

https://www.interviewbit.com/problems/unique-binary-search-trees-ii/

Given A, how many structurally unique BST's (binary search trees) that store values 1...A?

Example :

Given A = 3, there are a total of 5 unique BST's.

```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

## Hint 1

Can you compute the answer for A = n if you know the answer for A = 1, A = 2, ... A = n-1 ?

What values can you place at root first and how will it affect the answer? Try to think of DP.

## Solution Approach

Lets say you know the answer for values i which ranges from 0 <= i <= n - 1.

How do you calculate the answer for n.

Lets consider the number [1, n]

We have n options of choosing the root. 

If we choose the number j as the root, j - 1 numbers fall in the left subtree, n - j numbers fall in the right subtree. We already know how many ways there are to forming j - 1 trees using j - 1 numbers and n -j numbers. 

So we add number(j - 1) * number(n - j) to our solution.

Can you use the above fact to construct a DP relation ?

## Solution


### Editorial
```cpp
int Solution::numTrees(int n) {
    if (n == 0) return 1;
    if (n == 1) return 1;

    int result[n + 1];
    memset(result, 0, sizeof(result));
    result[0] = 1;
    result[1] = 1;
    if (n < 2) {
        return result[n];
    }

    for (int i = 2; i <= n; i++) {
        for (int k = 1; k <= i; k++) {
            result[i] = result[i] + result[k - 1] * result[i - k];
        }
    }

    return result[n];
}
```

### Lightweight
```cpp
int Solution::numTrees(int A) {
    int dp[A+1];
    dp[0] = 1;
    for(int i=1; i<=A;i++){
        dp[i] = 0;
        for(int j=1; j<=i; j++)
            dp[i] += dp[j-1]*dp[i-j];
    }
    return dp[A];
}
```

## Asked in
