# Stairs

https://www.interviewbit.com/problems/stairs/

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Example :

```
Input : 3
Return : 3
Steps : [1 1 1], [1 2], [2 1]
```

## Solution
```cpp
int Solution::climbStairs(int A) {
    if (A<2)
        return 1;
    vector<int> dp(A+1);
    dp[0] = 1; // starting point, 1 way
    dp[1] = 1; // *_1_*
    dp[2] = 2; // *_1_*_1_* or *__2__*
    for (int i=3; i<=A; i++)
        dp[i] = dp[i-2] + dp[i-1];
    return dp[A];
}
```

## Asked in

* Morgan Stanley
* Amazon
* Intel

