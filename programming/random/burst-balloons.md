# Burst Balloons

https://www.interviewbit.com/problems/burst-balloons/


You are given N ballons each with a number of coins associated with them.
An array of integers A represents the coins associated with the ith ballon.
You are asked to burst all the balloons.
If the you burst balloon ith you will get A[left] * A[i] * A[right] coins.
Here left and right are adjacent indices of i.
After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

### Note

You may imagine A[-1] = A[n] = 1. They are not real therefore you can not burst them.

Your submission will run on multiple test cases if you are using global make sure to clear them.


### Input Format

The only argument given is the integer array A.

### Output Format

Return the maximum coins you can collect by bursting the balloons wisely.

### Constraints

```
1 <= N <= 100
1 <= A[i] <= 100 
```

### For Example
```
Input 1:
    A = [3, 1, 5, 8]
Output 1:
    167
    Explanation 1:
        Burst ballon at index 1, coins collected = 3*1*5=15 , A becomes = [3, 5, 8]
        Burst ballon at index 1, coins collected = 3*5*8=120 , A becomes = [3, 8]
        Burst ballon at index 0, coins collected = 1*3*8=24 , A becomes = [8]
        Burst ballon at index 0, coins collected = 1*8*1 = 8
        Total coins collected = 15 + 120 + 24 + 8 = 167

Input 2:
    A = [1, 2, 3, 4, 5]
Output 2:
    110
```

## Hint 1
In the first try, the obvious solution is to find every possible order in which balloons can be burst.
This lead to time complexity of O(N!). We can improve further by caching the set of existing balloons.
Since each balloon can be burst or not burst, and
we are incurring extra time creating a set of balloons each time, we are still looking at a solution worse than O(2^N).
Try to Think where we can apply dynamic programming here.

## Solution appriach
We define a function dp to return the maximum number of coins obtainable on the open interval (left, right).
Our base case is if there are no integers on our interval (left + 1 == right), and
therefore there are no more balloons that can be added.
We add each balloon on the interval, divide and conquer the left and right sides, and find the maximum score.

The best score after adding the ith balloon is given by:

`A[left] * A[i] * A[right] + dp(left, i) + dp(i, right)`, where
`A[left] * A[i] * A[right]` is the number of coins obtained from adding the ith balloon,
and `dp(left, i) + dp(i, right)` are the maximum number of coins obtained from solving the left and right sides of that balloon respectively.

## Solution
### Editorial
```cpp
int dp[105][105];
int rec(int i,int j,vector<int> &A){
        if(j-i<=1)
            return 0;
        auto &ans=dp[i][j];
        if(ans!=-1)
            return ans;
        ans=0;
        for(int k=i+1; k<=j-1; ++k)
            ans=max(ans,rec(i,k,A)+rec(k,j,A)+A[i]*A[k]*A[j]);
        return ans;
}
int maxCoins(vector<int>& A) {
    A.insert(A.begin(), 1);
    A.push_back(1);
    int n=A.size();
    for(int i=0;i<=n; ++i)
        for(int j=0; j<=n; ++j)
            dp[i][j]=-1;
    int ans=rec(0,n-1,A);
    assert(ans<=INT_MAX);
    return ans;
}

int Solution::solve(vector<int> &A) {
    return maxCoins(A);
}
```

### Fastest
```cpp
int Solution::solve(vector<int> &nums) {
        nums.insert(nums.begin(), 1);
        nums.push_back(1);
        
        vector<vector<int>> dp(nums.size(), vector<int>(nums.size()));
        
        for (int len=1; len<nums.size()-1; len++) {
            for (int i=1; i+len-1<nums.size()-1; i++) {
                int s = 0;
                for (int k=i; k<=i+len-1; k++)
                    s = max(s, nums[i-1] * nums[k] * nums[i+len] + dp[i][k-1] + dp[k+1][i+len-1]);
                dp[i][i+len-1] = s;
            }
        }
        return dp[1][nums.size()-2];
    
}
```
### Lightweight
```cpp
int Solution::solve(vector<int> &A) {
    int n = A.size();
    int dp[n][n];
    memset(dp, 0, sizeof(dp));
    for(int len = 1;len<=n;len++)
    {
        for(int i=0;i<=n-len;i++)
        {
            int j = i+len-1;
            for(int k=i;k<=j;k++)
            {
                int lvalue=1, rvalue=1;
                if(i!=0)
                    lvalue = A[i-1];
                if(j!=n-1)
                    rvalue = A[j+1];
                int before = 0, after = 0;
                if(i!=k)
                    before = dp[i][k-1];
                if(j!=k)
                    after = dp[k+1][j];
                dp[i][j] = max(lvalue*A[k]*rvalue + before+after, dp[i][j]);
            }
        }
    }
    
    
    return dp[0][n-1];
}
```

### Mine
```python
class Solution:
    def solve(self, nums):
        if not nums:
            return 0
        nums = [1] + nums + [1]
        n = len(nums)
        dp = [[0]*n for _ in range(n)]
        for l in range(1, n+1):
            for i in range(1, n-l):
                j = i + l - 1
                for k in range(i, j+1):
                    dp[i][j] = max(dp[i][j], nums[i-1]*nums[k]*nums[j+1]+dp[i][k-1]+dp[k+1][j])
        return dp[1][n-2]
```

## References

* https://leetcode.com/problems/burst-balloons/

