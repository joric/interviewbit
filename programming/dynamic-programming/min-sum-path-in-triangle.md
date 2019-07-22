# Min Sum Path in Triangle

https://www.interviewbit.com/problems/min-sum-path-in-triangle/

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

### Note

Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle. 

## Hint 1

Brute force: Try traversing each possible path from top to the leaves. Not an acceptable solution in this case.

The triangle has a tree-like structure, which would lead people to think about traversal algorithms such as DFS. However, if you look closely, you would notice that the adjacent nodes always share a 'branch'. In other word, there are overlapping subproblems. Also, suppose x and y are 'children' of k. Once minimum paths from x and y to the bottom are known, the minimum path starting from k can be decided in O(1), that is optimal substructure. Therefore, dynamic programming would be the best solution to this problem in terms of time complexity.

## Solution Approach

For 'top-down' DP, starting from the node on the very top, we recursively find the minimum path sum of each node. When a path sum is calculated, we store it in an array (memoization); the next time we need to calculate the path sum of the same node, just retrieve it from the array. However, you will need a cache that is at least the same size as the input triangle itself to store the pathsum, which takes O(N^2) space. With some clever thinking, it might be possible to release some of the memory that will never be used after a particular point, but the order of the nodes being processed is not straightforwardly seen in a recursive solution, so deciding which part of the cache to discard can be a hard job.

'Bottom-up' DP, on the other hand, is very straightforward: we start from the nodes on the bottom row; the min pathsums for these nodes are the values of the nodes themselves. From there, the min pathsum at the ith node on the kth row would be the lesser of the pathsums of its two children plus the value of itself, i.e.:

minpath[k][i] = min( minpath[k+1][i], minpath[k+1][i+1]) + triangle[k][i];

Or even better, since the row minpath[k+1] would be useless after minpath[k] is computed, we can simply set minpath as a 1D array, and iteratively update itself:

For the kth level:

minpath[i] = min( minpath[i], minpath[i+1]) + triangle[k][i];

## Solution

### Editorial
```cpp
int Solution::minimumTotal(vector<vector<int>> &triangle) {
    int n = triangle.size();
    vector<int> minlen(triangle.back());
    for (int layer = n - 2; layer >= 0; layer--) // For each layer
    {
        for (int i = 0; i <= layer; i++) // Check its every 'node'
        {
            // Find the lesser of its two children, and sum the current value in the triangle with it.
            minlen[i] = min(minlen[i], minlen[i + 1]) + triangle[layer][i];
        }
    }
    return minlen[0];
}
```

### Fastest
```cpp
int dp[1024][1024];
int Solution::minimumTotal(vector<vector<int> > &A) {
    dp[0][0]=A[0][0];
    for(int i=1;i<A.size();i++){
        int s=A[i].size();
        dp[i][0]=dp[i-1][0]+A[i][0];
        dp[i][i]=dp[i-1][i-1]+A[i][i];
        for(int j=1;j<i;j++){
            dp[i][j]=min(dp[i-1][j-1],dp[i-1][j])+A[i][j];
        }
        /*for(int j=0;j<=i;j++)
            printf("%d %d %d\t",i,j,dp[i][j]);
        printf("\n");*/
    }
    int sol=1000000007;
    for(int j=0;j<A.size();j++){
        sol=min(sol,dp[A.size()-1][j]);
    }
    return(sol);
}
```

### Lightweight
```cpp
int Solution::minimumTotal(vector<vector<int> > &tri) {
        int row,col;
        int i,j;
        row=tri.size();
        //cout<<row<<endl;
        if(row==0)
            return 0;
        if(row==1)
            return tri[0][0];
            
        col=tri[0].size();
        
        for(i=row-2;i>=0;i--)
        {
            for(j=0;j<=i;j++)
            {
                tri[i][j]=tri[i][j] + min(tri[i+1][j],tri[i+1][j+1]);
            }
        }
        return tri[0][0];
}
```

### Mine
```cpp
int Solution::minimumTotal(vector<vector<int>> &A) {
    if (A.size() == 0)
        return 0;
    int total[A.size()];
    int l = A.size() - 1;
    for (int i = 0; i < A[l].size(); i++)
        total[i] = A[l][i];
    for (int i = A.size() - 2; i >= 0; i--)
        for (int j = 0; j < A[i + 1].size() - 1; j++)
            total[j] = A[i][j] + min(total[j], total[j + 1]);
    return total[0];
}

```

## Asked in


