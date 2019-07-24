# Divisor Sequences

https://www.interviewbit.com/problems/divisor-sequences/

Given a positive integer A, 
find and return the length of the longest sequence a[1], ..., a[k] of positive integers such that:

```
The sum of all a[i] is A.
For each valid i, a[i] < a[i+1].
For each valid i, a[i] divides a[i+1].

Input Format

The only argument given is integer A.
Output Format

Return the length of the longest sequence satisfying the above conditions.
Constraints

1 <= A <= 2*10^9
For Example

Input 1:
    A = 15
Output 1:
    4   (1, 2, 4, 8)

Input 2:
    A = 34
Output 2:
    4   (1, 3, 6, 24)
```

## Solution

### Editorial
```cpp
#define REP(i,n) for (int i = 0; i < n; i++)
#define remax(a,b) a = max(a,b)
#define all(v) v.begin(),v.end()
typedef map<int,int> mii;

int dp[10000005];
mii dpm;
 
int f(int x){
  if(x <= 1) return 0;
  if(x <= 10000000 and dp[x] != -1) return dp[x];
  if(x > 10000000 and dpm.find(x) != dpm.end()) return dpm[x];
  int res = 1;
  for(int d = 2; d*d <= x; d ++){
    if(x%d == 0){
      remax(res,1+f((x-d)/d));
      remax(res,1+f((x-(x/d))/(x/d)));
    }
  }
  if(x <= 10000000) return dp[x] = res;
  return dpm[x] = res;
}

int Solution::solve(int N) {
    REP(i,10000005) dp[i] = -1;
    return max(f(N),1+f(N-1));
}

```

### Fastest
```cpp
int f(int x, unordered_map<int,int>& memo) {
    if(x<2)return 0;
    if(memo.count(x))return memo[x];
    int res = 1;
    for (int d = 2; d*1LL*d<=x;++d) if (x%d==0) {
        int r = 1 + max(f(x/d-1, memo), f(d-1, memo));
        if(r>res)res=r;
    }
    return memo[x]=res;
}

int Solution::solve(int A) {
    if(A<3)return 1;
    unordered_map<int,int> memo;
    return max(f(A, memo), 1 + f(A-1, memo));
}

```

### Lightweight
```cpp
unordered_map<int,int> dp;
int rec(int A){
    //cout<<A<<endl;
    int res=1;
    if(A==1) return 1;
    if(A==2) return 1;
    if(A==3) return 2;
    if(A==4) return 2;
    
   // if(A==0) return 0;
   if(dp.find(A)!=dp.end()) return dp[A];
    for(int d=2;d<=ceil(sqrt(A));d++){
        if(A%d==0){
            int a=A/d;
            int b=d;
            res=max(res,rec(A/a));
            res=max(res,rec(A/b));
        }else if((A-1)%d==0){
            int a=(A-1)/d;
            int b=d;
            res=max(res,1+rec((A-1)/a));
            res=max(res,1+rec((A-1)/b));
        }
    }
  //  cout<<A<<": "<<res<<endl;
    return dp[A]=res;
}
int Solution::solve(int A) {
    dp.clear();
    return rec(A);
}
```
