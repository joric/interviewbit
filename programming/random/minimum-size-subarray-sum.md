# Minimum Size Subarray Sum

https://www.interviewbit.com/problems/minimum-size-subarray-sum/

Given an array A of n positive integers and a positive integer B,

find the minimal length of a contiguous subarray of which the sum >= B.

If there isn’t one, return 0 instead.

### NOTE

Your code may be run on multiple testcases ( <= 50 ). Try to come up with an optimised solution.

### CONSTRAINTS
```
1 <= n (size of array A) <= 10^5
1 <= A[i] <= 10^4
1 <= B <= 2*10^9
```
### INPUT FORMAT
```
A : Array containing some integer elements
B : The number by which the sum of contiguous sub-array should be greater
```
### SAMPLE INPUT
```
A : [ 2, 3, 1, 2, 4, 3 ]
B : 7
```
### SAMPLE OUTPUT
```
2
```
### EXPLANATION

The smallest possible sub array with sum >= 7 is [4,3]

## Solution
### Mine
```cpp
int Solution::solve(vector<int> &A, int B) {
    int res = INT_MAX;
    int sum = 0;
    for (int i = 0, j = 0; i < A.size(); i++) {
        sum += A[i];
        while (sum >= B) {
            res = min(res, i + 1 - j);
            sum -= A[j++];
        }
    }
    return res == INT_MAX ? 0 : res;
}
```
