# Intersecting Chords in a Circle

https://www.interviewbit.com/problems/intersecting-chords-in-a-circle/

Given a number N, return number of ways you can draw N chords in a circle with 2 * N points
such that no 2 chords intersect.

Two ways are different if there exists a chord which is present in one way and not in other.

For example,

```
N=2
If points are numbered 1 to 4 in clockwise direction, then different ways to draw chords are:
{(1-2), (3-4)} and {(1-4), (2-3)}

So, we return 2.
```
Notes:

1 <= N <= 1000
Return answer modulo 10^9 + 7.

## Hint 1
Think in terms of DP.
Can we relate answer for N with smaller answers.

If we draw a chord between any two points, can you observe current set of points getting broken into two smaller sets? Can a chord be drawn between two points where each point belong to different set?

## Solution Approach

Think in terms of DP.
Can we relate answer for N with smaller answers.

If we draw a chord between any two points, can you observe current set of points getting broken
into two smaller sets S_1 and S_2? Can a chord be drawn between two points where each point belong
to different set?

If we draw a chord from a point in S_1 to a point in S_2, it will surely intersect the chord we've just drawn.

So, we can arrive at a recurrence that Ways(n) = sum[i = 0 to n-1] { Ways(i)*Ways(n-i-1) }.

Here we iterate over i, assuming that size of one of the sets is i and size of other set automatically
is (n-i-1) since we've already used a pair of points and i pair of points in one set.


## Solution

### Editorial
```cpp
#define ull unsigned long long int
#define mod 1000000007
int Solution::chordCnt(int A) {
    vector<ull> cat(A+1);
    cat[0]=1;
    cat[1]=1;
    for(int i=2;i<=A;i++) {
        cat[i]=0;
        for(int j=0;j<i;j++) {
            cat[i]+=(((cat[j]%mod)*(cat[i-j-1]%mod)))%mod;
        }
    }
    return (int)(cat[A]%mod);
}
```

### Mine
```cpp
//The pattern gives Catalan numbers 1,1,2,5,14...
//So the solution is to find nth catalan number.

int Solution::chordCnt(int n){
    long long int dp[n+1];
   // if(n%2!=0) return 0;
    if(n==0||n==1)
     return 1;
    else if(n==2)
     return 2;
    else if(n>2){
        dp[0]=1;
        dp[1]=1;
        dp[2]=2;
        for(int i=3;i<=n;i++){
            dp[i]=0;
            for(int k=0;k<i;k++){
                dp[i]=(dp[i]+dp[i-k-1]*dp[k])%1000000007;
            }
        }
       
    }
     return dp[n]%1000000007;
}

```
