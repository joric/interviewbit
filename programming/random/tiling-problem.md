# Tiling Problem

https://www.interviewbit.com/problems/tiling-problem/

Given a 2 x A board and tiles of size 2 x 1.

Count the number of ways to tile the given board using the 2 x 1 tiles. 
A tile can either be placed horizontally i.e., as a 1 x 2 tile or vertically i.e., as 2 x 1 tile.

Since the number of ways can be very large,
return the number of ways modulo (1e9+7).

### Input Format

The only argument given is an integer A.

### Output Format

Return the number of ways to tile the given board modulo (10^9+7).

### Constraints

1 <= A <= 10^6

### For Example

```
Input 1:
    A = 3
Output 1:
    3
    Explanation 1:
        We need 3 tiles to tile the board of size  2 x 3. 
        We can tile the board using following ways
            1. Place all 3 tiles vertically. 
            2. Place first tile vertically and remaining 2 tiles horizontally.
            3. Place first 2 tiles horizontally and remaining tiles vertically

Input 2:
    A = 4
Output 2:
    5
    Explanation 2:
        For a 2 x 4 board, there are 5 ways
            1. All 4 vertical
            2. All 4 horizontal
            3. First 2 vertical, remaining 2 horizontal
            4. First 2 horizontal, remaining 2 vertical
            5. Corner 2 vertical, middle 2 horizontal
```

## Solution Approach

let Dp[N] be the number of ways to place tiles on a 2 x N board.
```
Dp[1] = 1 (place 1 tile vertically) 
Dp[2] = 2 (place 2 tile horizontally)
```
If we place first tile vertically, the problem reduces to Dp[N-1].If we place first tile horizontally, we have to place second tile also horizontally. So the problem reduces to Dp[N-2].

Now the recurrence relation is as follows
```
Dp[N] = Dp[N-1] + Dp[N-2]
```
It is simple linear dp.

BONUS : try to solve the problem in O(log N) time complexity.

## Solution
### Editorial
```cpp
int Solution::solve(int A) {
    vector<long long int>ar(A+1);
    int mod=1000000007;
    ar[1]=1; ar[2]=2; if(A<3) return A;
    for(int i=3;i<=A;i++){
        ar[i]=((ar[i-1]%mod)+(ar[i-2]%mod))%mod;
    }
    return ar[A];
}
```

### Fastest
```cpp
int tc=0;
const int mx=1000005;
const int mod=1000000007;
long long dp[mx];

void pre(){
    if(tc)
        return;
    tc=1;
    dp[1]=1;
    dp[2]=2;
    for(int i=3; i<mx; ++i)
        dp[i]=(dp[i-1]+dp[i-2])%mod;
}

int Solution::solve(int A) {
    pre();
    return dp[A];
}
```

### Lightweight
```cpp
#define M 1000000007 
int Solution::solve(int A) {
    long long int p=1,q=2,r;
    if(A<=2) return A;
    for(int i=3;i<=A;i++)
    {
        r=(p+q)%M;
        p=q%M;
        q=r%M;
    }
    return r;
}
```

### Mine
```cpp
int Solution::solve(int n) {
    if (n<=2) return n;
    long long a = 1, b = 2, c, mod=1e9+7;
    for (int i=3; i<=n; i++) {
        c = (a+b) % mod;
        a = b % mod;
        b = c % mod;
    }
    return c;
}
```
