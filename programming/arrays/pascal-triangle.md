# Pascal Triangle

https://www.interviewbit.com/problems/pascal-triangle/

Given numRows, generate the first numRows of Pascal's triangle.

Pascal's triangle : To generate A[C] in row R, sum up A'[C] and A'[C-1] from previous row R - 1.

Example:

Given numRows = 5,

Return

```
[
     [1],
     [1,1],
     [1,2,1],
     [1,3,3,1],
     [1,4,6,4,1]
]
```

## Hint 1

Did you account for base cases like numRows = 0, numRows = 1?

Alright, once we have that out of the window, let us look at how we can approach this problem.

Notice that the first and last numbers in each row ( for row >= 2 ) are 1 and 1.

For all the other numbers:

num at position i = number at position i in prev row + number at position (i + 1) in previous row.

## Solution Approach


num at position i = number at position i in prev row + number at position (i + 1) in previous row.

Now, note that to calculate num at position i, we need the numbers in previous row. Which means it makes sense to create rows in order.

Create a 2D matrix where Matrix[r] denotes row r. 
Now process the rows starting from row number 1.

Row number 1 is obviously just 1.

For Row i, Row[i][0] = Row[i][i] = 1. And Row[i][j] = Row[i-1][j] + Row[i-1][j-1], when j belongs to [1, i)


## Solution

```cpp
vector<vector<int> > Solution::solve(int A) {
     vector<vector<int>> r(A);

     for (int i = 0; i < A; i++) {
         r[i].resize(i + 1);
         r[i][0] = r[i][i] = 1;

         for (int j = 1; j < i; j++)
             r[i][j] = r[i - 1][j - 1] + r[i - 1][j];
     }

     return r;
}

/*
vector<vector<int> > Solution::solve(int n) {
    vector<vector<int>> a(n);
    
    for (int i=0; i<n; i++)
        a[i].push_back(1);
    
    for (int i=1; i<n; i++) {
        a[i].resize(i+1);
        for (int j=1; j<i; j++)
            a[i][j] = a[i-1][j-1] + a[i-1][j];
        a[i][i] = 1;
    }
    
    return a;
}
*/
```
