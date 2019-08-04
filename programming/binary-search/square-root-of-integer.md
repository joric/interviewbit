# Square Root Of Integer

https://www.interviewbit.com/problems/square-root-of-integer


Square Root of Integer
Implement int sqrt(int x).
Compute and return the square root of x.
If x is not a perfect square, return floor(sqrt(x))


## Hint 1

Think about the answer of this  a particular number r less than floor(sqrt(x))?

Answer of the above problem as a function of r will look like `[1,1, ... 1,0,0 ... 0]`.

Can you use this fact to devise a solution now?

## Solution Approach

Think in terms of binary search.
Let us say S is the answer.
We know that `0 <= S <= x`.
Consider any random number r.
```
If r*r <= x, S >= r
If r*r > x, S < r.
```
Maybe try to run a binary search for S.


## Solution

### Editorial

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

### Mine
```cpp
int Solution::sqrt(int x) {
    if (x < 2) return x;
    int l = 1, r = x, res = 0;
    while (l <= r) {
        long m = r + (l - r) / 2;
        if (m * m == x)
            return m;
        else if (m * m < x)
            l = m + 1, res = m;
        else
            r = m - 1;
    }
    return res;
```

