# Kth Row of Pascal's Triangle

https://www.interviewbit.com/problems/kth-row-of-pascals-triangle/

Given an index k, return the kth row of the Pascal's triangle.

Pascal's triangle: To generate A[C] in row R, sum up A'[C] and A'[C-1] from previous row R - 1.

Example:

Input: k = 3

Return: [1,3,3,1]

NOTE: k is 0 based. k = 0, corresponds to the row [1]. 

Note:Could you optimize your algorithm to use only O(k) extra space?

## Hint 1

You just need to follow the formula given in statement.

It's easy to do it in time complexity of O(k^2).

You can solve the problem in O(k^2) space but can you see the formula again and try to figure out which extra space you can ignore.

## Solution Approach

Did you account for base cases like numRows = 0, numRows = 1 ?

Take a look at how we can approach this problem.

Notice that the first and last numbers in each row ( for row >= 2 ) are 1 and 1.

For all the other numbers:

num at position i = number at position i in prev row + number at position (i + 1) in previous row.

Also, notice that for a row, you only need the value in the previous rows.

The values in i-2 row do not matter.

As such, all you need is to maintain 2 vectors and alternate between them.


## Solution

```cpp

vector<int> Solution::getRow(int A) {
    vector<int> r(A+1);
    r[0] = 1;
    int a = 1;
    for(int i=1; i<=A; i++){
        a = (a*(A-i+1)/i);
        r[i] = a;
    }
    return r;
}

/*
vector<int> Solution::getRow(int A) {
    A+=1;
     vector<vector<int>> r(A);

     for (int i = 0; i < A; i++) {
         r[i].resize(i + 1);
         r[i][0] = r[i][i] = 1;

         for (int j = 1; j < i; j++)
             r[i][j] = r[i - 1][j - 1] + r[i - 1][j];
     }

     return r[A-1];
}
*/
```
