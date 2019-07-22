# Aggressive Cows

https://www.interviewbit.com/problems/aggressive-cows


Aggressive cows
Farmer John has built a new long barn, with N stalls. 
Given an array of integers A of size N where each element of the array represents the location of the stall, 
and an integer B which represent the number of cows.

His cows don't like this barn layout and become aggressive towards each other once put 
into a stall. To prevent the cows from hurting each other, John wants to assign the cows to the stalls, 
such that the minimum distance between any two of them is as large as possible. What is the largest minimum distance?

## Solution

```cpp

int cows(vector < int >&A, int x, int cows) {
    int n = A.size ();
    int cowsplaced = 1, lastpos = A[0];
    for (int i = 1; i < n; i++) {
        if (A[i] - lastpos >= x) {
            if (++cowsplaced == cows)
                return 1;
            lastpos = A[i];
        }
    }
    return 0;
}

int Solution::solve(vector<int> &A, int B) {
    sort (A.begin(), A.end());
    int n = A.size();
    int l = 0;
    int r = A[n-1] - A[0]+1;
    while (r-l>1) {
        int m = (l+r)/2;
        if (cows(A, m, B))
            l = m;
        else
            r = m;
    }
    return l;
}
```