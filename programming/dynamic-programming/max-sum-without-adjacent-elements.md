# Max Sum Without Adjacent Elements

https://www.interviewbit.com/problems/max-sum-without-adjacent-elements/

Given a 2 * N Grid of numbers, choose numbers such that the sum of the numbers
is maximum and no two chosen numbers are adjacent horizontally, vertically or diagonally, and return it.

Example:
```
Grid:
	1 2 3 4
	2 3 4 5
so we will choose
3 and 5 so sum will be 3 + 5 = 8
```

Note that you can choose more than 2 numbers


# Hint 1

No two adjacent elements should be taken ( Adjacent is defined by horizontally, vertically, diagonally ).

so suppose we have 2 * N list :

```
1 |  2  |  3  | 4
2 |  3  |  4  | 5

Now suppose we choose 2, then we can't choose the element just above it 1, 
    the element next it 3, or the element diagonally opposite. 
In other words, if we are on (x, y), then if we choose (x, y), we can't choose
(x + 1, y), (x, y + 1) and (x + 1, y + 1). 
```

Can you implement a brute force for this using recursion using the above fact ? 
Can you memoize the bruteforce recursive solution ? 

## Solution Approach

```
V : 
1 |  2  |  3  | 4
2 |  3  |  4  | 5

Lets first try to reduce it into a simpler problem. 
We know that within a column, we can choose at max 1 element. 
And choosing either of those elements is going to rule out choosing anything from the previous or next column. 
This means that choosing V[0][i] or V[1][i] has identical bearing on the elements which are ruled out. 
So, instead we replace each column with a single element which is the max of V[0][i], V[1][i].

Now we have the list as : 
2 3 4 5

Here we can see that we have reduced our problem into another simpler problem.
Now we want to find maximum sum of values where no 2 values are adjacent. 
Now our recurrence relation will depend only on position i and,
 a "include_current_element" which will denote whether we picked last element or not.
  
MAX_SUM(pos, include_current_element) = 
IF include_current_element = FALSE THEN   
	max | MAX_SUM(pos - 1, FALSE) 
	    | 
	    | MAX_SUM(pos - 1, TRUE)

ELSE    |
	MAX_SUM(pos - 1, FALSE) + val(pos) 
```


## Solution

### Editorial

```cpp
int Solution::adjacent(vector<vector<int> > V) {
    assert(V.size() == 2);
    int N = V[0].size();
    int MAXSUM[N + 1][2];
    memset(MAXSUM, 0, sizeof(MAXSUM));
    int ele = max(V[0][0], V[1][0]);
    MAXSUM[0][1] = ele;
    for (int i = 1; i < N; i++) {
        int cur_element = max(V[0][i], V[1][i]);
        MAXSUM[i][0] = max(MAXSUM[i-1][0], MAXSUM[i-1][1]);
        MAXSUM[i][1] = cur_element + MAXSUM[i-1][0];
    }
    return max(MAXSUM[N-1][0], MAXSUM[N-1][1]);
}
```

### Fastest

```cpp
int Solution::adjacent(vector<vector<int> > &A) {
    int dp[A[0].size()+1];
    dp[0]=0;
    dp[1]=max(A[0][0],A[1][0]);
    dp[2]=max(A[0][1],A[1][1]);
    for(int i=3;i<=A[0].size();i++) {
        dp[i]=max(A[0][i-1],A[1][i-1])+max(dp[i-2],dp[i-3]);
    }
    return max(dp[A[0].size()],dp[A[0].size()-1]);
}
```

### Mine

```cpp
int Solution::adjacent(vector<vector<int> > &A) {
    int n=A[0].size();
    int dp[2][n];
    if(n==0) return 0;
    if(n==1) return max(A[0][0],A[1][0]);
    int ans=INT_MIN;
    for(int i=0;i<2;i++){
     dp[i][n-1]=A[i][n-1];
     ans=max(ans,dp[i][n-1]);
    }
    dp[0][n-2]=A[0][n-2];
    dp[1][n-2]=A[1][n-2];
    ans=max(ans,max(dp[0][n-2],dp[1][n-2]));
    
    int i=1;
    for(int j=n-3;j>=0;j--){
        int maxfromright=INT_MIN;
        for(int k=j+2;k<n;k++){
            maxfromright=max(maxfromright,max(dp[0][k],dp[1][k]));
        }
        dp[i][j]=max(A[i][j],A[i][j]+maxfromright);
        dp[i-1][j]=max(A[i-1][j],A[i-1][j]+maxfromright);
        ans=max(ans,max(dp[i][j],dp[i-1][j]));
    }
    return ans;
}
```


