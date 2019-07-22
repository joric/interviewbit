# Anti-Diagonals

https://www.interviewbit.com/problems/anti-diagonals/

Give a N*N square matrix, return an array of its anti-diagonals. Look at the example for more details.

Example:

```
Input: 	

1 2 3
4 5 6
7 8 9

Return the following :

[ 
  [1],
  [2, 4],
  [3, 5, 7],
  [6, 8],
  [9]
]


Input : 
1 2
3 4

Return the following  : 

[
  [1],
  [2, 3],
  [4]
]
```

## Hint 1

Lets look at how the co-ordinates change when you move from one element to the other in the anti-diagonal.

With every movement, row increases by one, and the column decreases by one ( or in other words (1, -1) gets added to the current co-ordinates ).

Now, all we need to know is the start ( or the first element ) in each diagonal.

Can you figure out which elements qualify as the first elements in each diagonal ?

## Solution

```cpp

vector<vector<int> > Solution::diagonal(vector<vector<int> > &A) {
    vector<vector<int>> result;
    vector<int> diagonal;
    
    int n = A.size();
    if(n == 0)
        return result;
    for(int d = 0; d <= 2*(n-1); d++) {
       for(int i = 0; i <= d; i++) {
           int j = d - i;
           //continue if i or j exceeds their bounds
           if(i >= n || j >= n)
                continue;
           diagonal.push_back(A[i][j]);
       }
       result.push_back(diagonal);
       diagonal.clear();
   }
   return result;
}

/*
vector<vector<int> > Solution::diagonal(vector<vector<int> > &A) {
    int n = A.size();
    int N = 2*(n-1) + 1;//number of vectors in ans
    vector<vector<int>> ans(N);
    for(int i = 0;i<n;i++)
    {
        for(int j = 0;j<n;j++)
            ans[i+j].push_back(A[i][j]);//sum of index values in 2d array gives the index in ans
    }
    return ans;
}
*/
```
