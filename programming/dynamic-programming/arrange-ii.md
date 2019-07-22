# Arrange II

https://www.interviewbit.com/problems/arrange-ii/

You are given a sequence of black and white horses, and a set of K stables numbered 1 to K. You have to accommodate the horses into the stables in such a way that the following conditions are satisfied:

You fill the horses into the stables preserving the relative order of horses. For instance, you cannot put horse 1 into stable 2 and horse 2 into stable 1. You have to preserve the ordering of the horses.

No stable should be empty and no horse should be left unaccommodated.

Take the product (number of white horses * number of black horses) for each stable and take the sum of all these products. This value should be the minimum among all possible accommodation arrangements

### Example

```
Input: {WWWB} , K = 2
Output: 0

Explanation:
We have 3 choices {W, WWB}, {WW, WB}, {WWW, B}
for first choice we will get 1*0 + 2*1 = 2.
for second choice we will get 2*0 + 1*1 = 1.
for third choice we will get 3*0 + 0*1 = 0.

```

Of the 3 choices, the third choice is the best option. 

If a solution is not possible, then return -1

## Hint 1

Hint : Dynamic programming

Can we use the fact that the order should be preserved ? Could you create a recurrence relation using it ? 
You can have a state around [first X number of horses, first Y number of stables].

## Solution Approach

Recurrence relation

```
Rec(Current_Horse, Current_Stable) =  |   
                                      |           |
                                      |           |Rec(i + 1, Next_Stable) + (White_Horses * Black Horses in Current_Stable)  
                                      |       min |
                                      |           |
                                      |   
                                      | i = Current_Horse to Number_of_Horses  
                                      |      
```

Now can you implement it?

Happy Coding

## Solution

### Editorial
```cpp
vector<vector<int>> dp;

int rec(int start, int stables, string str, int K) {
    int N = str.size();
    if (start == N) {
        if (stables == K)
            return 0;
        return INT_MAX;
    }

    if (stables == K)
        return INT_MAX;

    if (dp[start][stables] != -1)
        return dp[start][stables];

    int W = 0;
    int B = 0;
    int ans = INT_MAX;

    for (int i = start; i < N; ++i) {
        W += str[i] == 'W';
        B += str[i] == 'B';
        if (W * B > ans) break;
        int Temp = rec(i + 1, stables + 1, str, K);
        if (Temp != INT_MAX) {
            ans = min(ans, Temp + (W * B));
        }
    }

    return dp[start][stables] = ans;
}

int Solution::arrange(string str, int K) {
    int N = str.size();
    dp.clear();
    dp.resize(N, vector<int>(K, -1));

    int ans = rec(0, 0, str, K);
    return ans == INT_MAX ? -1 : ans;
}

```

### Fastest
```cpp
int Solution::arrange(string A, int B) {
    
    if (A.size() < B)
        return -1;
    
    vector<int> ctWhite, cost, old;
    ctWhite.resize(A.size(), 0);
    cost.resize(A.size(), 0);
    old.resize(A.size(), 0);
    
    int curWhite = 0;
    for (int i = 0; i < A.size(); i++){
        if (A[i] == 'W')
            curWhite++;
            
        ctWhite[i] = curWhite;
        old[i] = ctWhite[i] * (i + 1 - ctWhite[i]);
    }
    
    int minIdx;
    for (int lev = 1; lev < B; lev++){
        minIdx = 0;
        for (int i = lev; i < A.size(); i++){
            
            int itvWhite = ctWhite[i] - ctWhite[minIdx];
            int curCost = old[minIdx] + itvWhite*(i - minIdx - itvWhite);
            
            int curIdx = minIdx;
            while (curIdx + 1 < i){
                itvWhite = ctWhite[i] - ctWhite[curIdx + 1];
                int tempCost = old[curIdx + 1] + itvWhite*(i - curIdx - 1 - itvWhite);
                if (tempCost < curCost){
                    minIdx = curIdx++;
                    curCost = tempCost;
                } else {
                    curIdx++;
                }
            }
            cost[i] = curCost;
            // cout << A[i] << "\t" << minIdx << " " << i << "\t" 
            //         << itvWhite << " " << (i - minIdx - itvWhite) << "\t" 
            //         << old[minIdx] << " " << cost[i] << endl;
        }
        old = cost;
        cost.clear();
        cost.resize(A.size(), 0);
    }
    
    return old[cost.size() - 1];
}
```

### Lightweight
```cpp
// spnauT
//
#include <bits/stdc++.h>
using namespace std;
#define FOR(i,a,b) for(int _b=(b),i=(a);i<_b;++i)
#define ROF(i,b,a) for(int _a=(a),i=(b);i>_a;--i)
#define REP(n) for(int _n=(n);_n--;)
#define _1 first
#define _2 second
#define PB(x) push_back(x)
#define SZ(x) int((x).size())
#define ALL(x) (x).begin(),(x).end()
#define MSET(m,v) memset(m,v,sizeof(m))
#define MAX_PQ(T) priority_queue<T>
#define MIN_PQ(T) priority_queue<T,vector<T>,greater<T>>
#define IO(){ios_base::sync_with_stdio(0);cin.tie(0);}
#define nl '\n'
#define cint1(a) int a;cin>>a
#define cint2(a,b) int a,b;cin>>a>>b
#define cint3(a,b,c) int a,b,c;cin>>a>>b>>c
typedef long long LL;typedef pair<int,int> PII;
typedef vector<int>VI;typedef vector<LL>VL;typedef vector<PII>VP;
template<class A,class B>inline bool mina(A &x,const B &y){return(y<x)?(x=y,1):0;}
template<class A,class B>inline bool maxa(A &x,const B &y){return(x<y)?(x=y,1):0;}

int Solution::arrange(string A, int B)
{
    int N = SZ(A);
    if(N < B) return -1;
    
    VI C(N+1);
    C[0] = 0;
    FOR(j,0,N) C[j+1] = C[j] + (A[j] == 'B');
    VL dp[2];
    int i0 = 0;
    int i1 = 1;
    dp[0].resize(N+1,INT_MAX);
    dp[0][0] = 0;
    REP(B)
    {
        dp[i1].resize(N+1,INT_MAX);
        FOR(i,0,N) if(dp[i0][i] < INT_MAX) FOR(j,i+1,N+1)
        {
            int a = C[j] - C[i];
            mina(dp[i1][j], dp[i0][i] + a*(j-i-a));
        }
        swap(i0,i1);
    }
    return dp[i0][N];
}
```

## Asked in
* Amazon
