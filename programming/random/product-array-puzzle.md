# Product array puzzle

https://www.interviewbit.com/problems/product-array-puzzle/

Given an array of integers A, find and return the product array of same size where
i'th element of the product array will be equal to the product of all the elements divided by the i'th element of the array.

Note: It is always possible to form the product array with integer (32 bit) values.

### Input Format

The only argument given is the integer array A.

### Output Format

Return the product array.

### Constraints

```
1 <= length of the array <= 1000
1 <= A[i] <= 10
```

### For Example
```
Input 1:
    A = [1, 2, 3, 4, 5]
Output 1:
    [120, 60, 40, 30, 24]

Input 2:
    A = [5, 1, 10, 1]
Output 2:
    [10, 50, 5, 50]
```

## Solution
### Mine (passes tests)
```cpp
vector<int> Solution::solve(vector<int> &A) {
    long long prod = 1;
    for (int x:A) prod *= x;
    vector<int> res;
    for (int x:A) res.push_back(prod/x);
    return res;
}
```

### Optimized (integers)
```cpp
vector<int> Solution::solve(vector<int> &a) {
    int n = a.size();
    vector<int> l(n); l[0] = 1;
    for (int i = 1; i<n; i++) l[i] = a[i-1] * l[i-1];     
    vector<int> r(n); r[n-1] = 1;
    for (int i = n-2; i>=0; i--) r[i] = a[i+1] * r[i+1]; 
    vector<int> res(n);
    for (int i = 0; i < n; i++) res[i] = l[i] * r[i]; 
    return res;
}

```

