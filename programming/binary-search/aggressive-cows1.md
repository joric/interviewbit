# Aggressive Cows1

https://www.interviewbit.com/problems/aggressive-cows1


Aggressive cows
Farmer John has built a new long barn, with N stalls. 
Given an array of integers A of size N where each element of the array represents the location of the stall, 
and an integer B which represent the number of cows.

His cows don't like this barn layout and become aggressive towards each other once put 
into a stall. To prevent the cows from hurting each other, John wants to assign the cows to the stalls, 
such that the minimum distance between any two of them is as large as possible. What is the largest minimum distance?

https://www.interviewbit.com/problems/aggressive-cows/

https://www.quora.com/What-is-the-correct-approach-to-solve-the-SPOJ-problem-Aggressive-cow/answer/Raziman-T-V

F(x) = 1 if it is possible to arrange the cows in stalls such that the distance between any two cows is at least x
F(x) = 0 otherwise

Now it is easy to see that if F(x)=0, F(y)=0 for all y>x. Thus, the problem satisfies the monotonicity condition necessary for binary search. It is also easy to find two values of x which give F(x)=0 and 1 respectively. F(0)=1 trivially since the distance between any two cows is at least 0. 

Now, how do we check whether F(x)=1 for a general value of x? We can do this with a greedy algorithm: Keep placing cows at the leftmost possible stalls such that they are at least x distance away from the last placed cow. 

## Solution

```cpp

int cows(vector<int> &A, int x, int cows) {
    int n = A.size();
    int count = 1;
    int last = A[0];
    for (int i = 1; i < n; i++) {
        if (A[i] - last >= x) {
            if (++count == cows)
                return 1;
            last = A[i];
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