# Maximum Unsorted Subarray

https://www.interviewbit.com/problems/maximum-unsorted-subarray/

You are given an array (zero indexed) of N non-negative integers, A[0], A[1] , ... , A[N-1].
Find the minimum sub array A[l], A[l+1] , ... , A[r] so if we sort(in ascending order) that sub array,
then the whole array should get sorted. If A is already sorted, output -1.

Example :
```
Input 1:

A = [1, 3, 2, 4, 5]

Return: [1, 2]

Input 2:

A = [1, 2, 3, 4, 5]

Return: [-1]
```

In the above example(Input 1), if we sort the subarray A1, A2, then whole array A should get sorted.

## Hint 1

Assume that A[l], ..., A[r] is the minimum-unsorted-subarray which is to be sorted,
then subarrays A[0], ... , A[l-1] and A[r+1], ... , A[N-1] will be in sorted(ascending) order.

How would you solve now?

## Solution Approach

Assume that A[l], ..., A[r] is the minimum-unsorted-subarray which is to be sorted.
then min(A[l], ..., A[r]) >= max(A[0], ..., A[l-1])
and max(A[l], ..., A[r]) <= min(A[r+1], ..., A[N-1])

How would you solve now?

## Solution

```cpp
vector < int >Solution::subUnsort (vector < int >&arr) {
    
    int n = arr.size();
    int s = 0, e = n - 1, i, max, min;

    // step 1(a) of above algo 
    for (s = 0; s < n - 1; s++) {
        if (arr[s] > arr[s + 1])
            break;
    }
    if (s == n - 1) {
        // The complete array is sorted"
        return {-1};
    }

    // step 1(b) of above algo 
    for (e = n - 1; e > 0; e--) {
        if (arr[e] < arr[e - 1])
            break;
    }

    // step 2(a) of above algo 
    max = arr[s];
    min = arr[s];
    for (i = s + 1; i <= e; i++) {
        if (arr[i] > max)
            max = arr[i];
        if (arr[i] < min)
            min = arr[i];
    }

    // step 2(b) of above algo 
    for (i = 0; i < s; i++) {
        if (arr[i] > min) {
            s = i;
            break;
        }
    }

    // step 2(c) of above algo 
    for (i = n - 1; i >= e + 1; i--) {
        if (arr[i] < max) {
            e = i;
            break;
        }
    }

    return {s, e};
}
```

## Asked in

* Amazon

