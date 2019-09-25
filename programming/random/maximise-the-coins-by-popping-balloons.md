# Maximise the coins by popping balloons

https://www.interviewbit.com/problems/maximise-the-coins-by-popping-balloons/


Given N balloons, indexed from 0 to N - 1. Each balloon is painted with a number on it represented by array A.

You are asked to burst all the balloons. If you burst the balloon i you will get A[left] x A[i] x A[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

### NOTE

You may imagine A[-1] = A[N] = 1. They are not real. Therefore, you can not burst them.

### Input Format

First and only argument of input contains an integer array A of size N.

### Output Format

Return a single integer representing the maximum coins.

### Constraints
```
1 <= N <= 500
0 <= A[i] <= 100
```
### For Example
```
Input 1:
    A = [2,3]
Output 1:
    9
Explanation 1:
    it is better to burst 1 followed by 2 for a total of 9 coins.

Input 2:
    A = [2,4,3,2] 
Output 2:
    46
```

## Hint 1

Think of a dynamic programming approach. Can you find overlapping and independent states using divide and conquer?

## Solution Approach

In this problem it is better to think from the last balloon popped because last balloon popped will always give A[-1]A[i]A[n-1] coins.
Now extend when this ballon is popped there are two sub disjoint and independent subarray. you can easily use dynamic programming for that.
Time complexity of the solution will be O(N^3)


## Solution
### Editorial

```cpp
int solve(vector<vector<int> > &dp,vector<int> &ar,int l,int r)
{
    if(l>=r+1)
        return 0;
    if(dp[l][r]>0)
        return dp[l][r];
    int ans=0;
    for(int i=l+1;i<r;i++)
    {
        ans=max(ans,ar[i]*ar[l]*ar[r]+solve(dp,ar,l,i)+solve(dp,ar,i,r));
    }
    dp[l][r]=ans;
    return ans;
}
int Solution::maxCoins(vector<int> &A) {
    vector<int> ar;
    int sz=0;
    ar.push_back(1);
    for(int a:A)
        if(a!=0)
            ar.push_back(a);
    ar.push_back(1);
    sz=ar.size();
    vector<vector<int> > dp(sz);
    for(int i=0;i<sz;i++)
        dp[i].resize(sz);
    return solve(dp,ar,0,sz-1);
}

```

### Fastest
```cpp
int Solution::maxCoins(vector<int> &A) {
    int n=A.size();
    A.insert(A.begin(),1);
    A.push_back(1);
    vector<vector<int>> dp(n+2,vector<int>(n+2,0));
    for(int len=1;len<=n;len++){
        for(int i=1;i<=n-len+1;i++){
            int j=i+len-1;
            for(int k=i;k<=j;k++){
                dp[i][j]=max(dp[i][j],dp[i][k-1]+A[i-1]*A[k]*A[j+1]+dp[k+1][j]);
            }
        }
    }
    return dp[1][n];
}
```
