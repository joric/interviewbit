# Coins in a Line

https://www.interviewbit.com/problems/coins-in-a-line/


There are N coins (Assume N is even) in a line. Two players take turns to take a coin from one of the ends of the line until there are no more coins left. The player with the larger amount of money wins. Assume that you go first.

Write a program which computes the maximum amount of money you can win.

Example:

```
suppose that there are 4 coins which have value
1 2 3 4
now you are first so you pick 4
then in next term
next person picks 3 then
you pick 2 and
then next person picks 1
so total of your money is 4 + 2 = 6
next/opposite person will get 1 + 3 = 4
so maximum amount of value you can get is 6
```

## Hint 1

For each turn, we have two possibilites : We can either pick from the start or the end.

Can you find a recurrance relation by using the above fact ? 
Think dynamic programming.

## Solution Approach

So, the question is what's the recurrence relation for this problem
```
Rec(player = you, start, end) = 
	    |
	max | v(end) + Rec(opposite_player, start, end - 1)  
	    |
	    | v(start) + Rec(opposite_player, start + 1, end)
	    |
Rec(player = opposite, start, end) = 
	    |
	min | Rec(you, start, end - 1)
	    |
	    | Rec(you, start + 1, end)
	    |
```
now can you define base cases and memorize it ?


## Solution
### Tutorial
```cpp
vector<vector<vector<int>>> dp;

int rec(int plyr, int start, int end, vector<int> vec) {
    if (start > end)
        return 0;
    if (dp[plyr][start][end] != -1)
        return dp[plyr][start][end];

    if (plyr == 0) {
        int ans = rec(1, start + 1, end, vec) + vec[start];
        ans = max(ans, rec(1, start, end - 1, vec) + vec[end]);
        return dp[plyr][start][end] = ans;
    } else {
        return dp[plyr][start][end] = min(rec(0, start + 1, end, vec), rec(0, start, end - 1, vec));
    }
}

int Solution::maxcoin(vector<int> & vec) {
    int N = vec.size();
    dp.clear();
    dp.resize(2, vector<vector<int>>(N, vector<int>(N, -1)));
    int ans = rec(0, 0, N - 1, vec);
    return ans;
}

```

### Mine
```cpp
int sum(vector<int> & ds, int st, int end) {
  return ds[end + 1] - ds[st];
}

int Solution::maxcoin(vector<int> & a) {
    int n = a.size();
    vector<int> ds(n+1);
    vector< vector<int> > dp(n+1, vector<int>(n));
    
    for (int i = 0; i < n; i++) {
        ds[i + 1] = ds[i] + a[i];
        dp[i][i] = a[i];
    }
    
    for (int k = 1; k < n; k++) {
        for (int i = 0; i < n - k; i++) {
            int j = i + k;
            dp[i][j] = sum(ds, i, j) - min(dp[i][j - 1], dp[i + 1][j]);
        }
    }
    return (int) dp[0][n - 1];
}
```
