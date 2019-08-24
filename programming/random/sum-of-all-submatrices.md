# Sum of all Submatrices

https://www.interviewbit.com/problems/sum-of-all-submatrices/

Given a 2D Matrix of dimensions N*N, we need to return sum of all possible submatrices.

### Example Input
```
[ [1,1],
  [1,1] ]
```
### Example Output
```
16
```

### Explanation

```
Number of submatrices with 1 elements = 4, so sum of all such submatrices = 4*1 = 4
Number of submatrices with 2 elements = 4, so sum of all such submatrices = 4*2 = 8
Number of submatrices with 3 elements = 0
Number of submatrices with 4 elements = 1, so sum of such submatrix = 4

Total Sum = 4+8+4 = 16
```
## Solution
### Mine
```cpp
int Solution::solve(vector<vector<int> > &arr) {
    int sum = 0, n = arr.size();
    for (int i = 0; i < n; i++) 
        for (int j = 0; j < n; j++) { 
            int top_left = (i + 1) * (j + 1); 
            int bottom_right = (n - i) * (n - j); 
            sum += (top_left * bottom_right * arr[i][j]); 
        }
    return sum;
}
```

