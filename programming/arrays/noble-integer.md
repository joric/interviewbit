# Noble Integer

https://www.interviewbit.com/problems/noble-integer/

Given an integer array, find if an integer p exists in the array such that the number of integers greater than p in the array equals to p
If such an integer is found return 1 else return -1.

## Hint 1

The straightforward approach is to for every element find how many integers are greater than that, and if that matches our given statement then we have our answer.

Will sorting the array help?

## Solution Approach

First we sort the input array.

Now, all we have to do is to traverse through each element of the array and check whether it matches our given statement, since the array is sorted we directly know how many elements are greater than that number in the array.

Note: Please take care of cases, when certain element repeats many times.

## Solution

```cpp
int Solution::solve(vector<int> &A) {
    sort(A.begin(), A.end());
    for (int i = 0; i<n-1; i++) {
        if (A[i] == A[i+1])
            continue;
        if (A[i] == n-i-1)
            return A[i];
    }
    if (A[n-1] == 0) 
        return A[n-1];
    return -1;
}
```
