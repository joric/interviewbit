# Square Root Of Integer

https://www.interviewbit.com/problems/square-root-of-integer


Square Root of Integer
Implement int sqrt(int x).
Compute and return the square root of x.
If x is not a perfect square, return floor(sqrt(x))


Think in terms of binary search.
Let us say S is the answer.
We know that 0 <= S <= x.
Consider any random number r.
If r*r <= x, S >= r
If r*r > x, S < r.
Maybe try to run a binary search for S.

## Solution

```cpp

int Solution::sqrt(int A) {
    if (A==0 || A==1) return A;
    int start = 0, end = A;
    int ans;
    while(start<=end) {
        int mid = start + (end - start)/2;
        if (mid <= A/mid) {
            start = mid + 1;
            ans = mid;
        }
        else 
            end = mid - 1;
    }
    return ans;
}

```