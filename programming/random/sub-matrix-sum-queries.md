# Sub-matrix Sum Queries

https://www.interviewbit.com/problems/sub-matrix-sum-queries/

Given a matrix of integers A of size N x M and multiple queries Q, for each query find and return the submatrix sum.

Inputs to queries are top left (b, c) and bottom right (d, e) indexes of submatrix whose sum is to find out.

Note: Rows are numbered from top to bottom and columns are numbered from left to right.

Sum may be large so return the answer mod 10^9 + 7.

### Input Format

* The first argument given is the integer matrix A.
* The second argument given is the integer array B.
* The third argument given is the integer array C.
* The fourth argument given is the integer array D.
* The fifth argument given is the integer array E.
* (B[i], C[i]) represents the top left corner of the i'th query.
* (D[i], E[i]) represents the bottom right corner of the i'th query.

### Output Format

Return the submatrix sum (% 10^9 + 7) for each query in the form of an integer array.

### Constraints

```
1 <= N, M <= 1000
-100000 <= A[i] <= 100000
1 <= Q <= 100000
1 <= B[i] <= D[i] <= N
1 <= C[i] <= E[i] <= M
```

### For Example
```
Input 1:
    A = [   [1, 2, 3]
            [4, 5, 6]
            [7, 8, 9]   ]
    B = [1, 2]
    C = [1, 2]
    D = [2, 3]
    E = [2, 3]
Output 1:
    [12, 28]

Input 2:
    A = [   [5, 17, 100, 11]
            [0, 0,  2,   8]    ]
    B = [1, 1]
    C = [1, 4]
    D = [2, 2]
    E = [2, 4]
Output 2:
    [22, 19]
```

## Solution 
### Editorial
```cpp
vector<int> Solution::solve(vector<vector<int> > &a, vector<int> &B, vector<int> &C,
			vector<int> &D, vector<int> &E) {
    int n = a.size(), m = a[0].size();
    int dp[n+1][m+1];
    for(int i = 0; i <= n; i++) {
        for(int j = 0; j <= m; j++) {
            if(i == 0 || j == 0)    dp[i][j] = 0;
            else    dp[i][j] = a[i-1][j-1] + dp[i-1][j] + dp[i][j-1] - dp[i-1][j-1];
        }
    }
    int q = B.size();
    vector <int> ans;
    for(int i = 0; i < q; i++) {
        int x1 = B[i] - 1, y1 = C[i] - 1;
        int x2 = D[i], y2 = E[i];
        assert(x1 < x2 && y1 < y2);
        ans.push_back(dp[x2][y2] - dp[x1][y2] - dp[x2][y1] + dp[x1][y1]);
    }
    return ans;
}
```

### Fastest
```cpp
vector<int> Solution::solve(vector<vector<int> > &A, vector<int> &B, vector<int> &C,
			vector<int> &D, vector<int> &E) {
    int n = A.size(), m = A[0].size();
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            if(i==0&&j==0){}
            else if(i==0){
                A[i][j] = A[i][j]+A[i][j-1];
            }
            else if(j==0){
                A[i][j] = A[i][j]+A[i-1][j];
            }
            else A[i][j] = A[i][j]+A[i-1][j]+A[i][j-1]-A[i-1][j-1];
        }
    }
    vector<int> arr;
    n = D.size();
    for(int k=0;k<n;k++){
        int r=A[D[k]-1][E[k]-1];
        int r1=0,r2=0,r3=0;
        if(B[k]-2>=0&&C[k]-2>=0){
            r1= A[B[k]-2][C[k]-2];
            r2= A[B[k]-2][E[k]-1];
            r3= A[D[k]-1][C[k]-2];
        }
        else if(B[k]-2>=0){
            r2= A[B[k]-2][E[k]-1];
        }
        else if(C[k]-2>=0) r3= A[D[k]-1][C[k]-2];
        arr.push_back(r+r1-r2-r3);
    }
    return arr;
}
```

### Lightweight
```cpp
vector<int> Solution::solve(vector<vector<int> > &A, vector<int> &B, vector<int> &C,
			vector<int> &D, vector<int> &E) 
{
   int n=A.size();
   int m=A[0].size();
   
  
    for(int i=0;i<n;i++)
    {
        int count=0;
        for(int j=0;j<m;j++)
        {
           // A[i][j]%=1000000007;
          A[i][j]=A[i][j]+(count);
          if(A[i][j]<0)
          {
              A[i][j]+=1000000007;
          }
          else
          {
              A[i][j]%=1000000007;
          }
          count=A[i][j];
          
        }
    }
    for(int i=0;i<m;i++)
    {
        int count=0;
        for(int j=0;j<n;j++)
        {
          // A[j][i]%=1000000007; 
          A[j][i]+=count;
          if(A[j][i]<0)
          {
              A[j][i]+=1000000007;
          }
          else
          {
              A[j][i]%=1000000007;
          }
          count=A[j][i];
          
        }
    }
    vector<int> f(B.size(),0);
    for(int i=0;i<B.size();i++)
    {
      int  a=B[i]-1;
        int b=C[i]-1;
        int c=D[i]-1;
        int d=E[i]-1;
        int l=A[c][d];
        
        int m=0;
        int n=0;
        int o=0;
        if(b!=0)
        {
        m=A[c][b-1];
        }
        else
        {
           m=0;
        }
        if(a!=0)
        {
        n=A[a-1][d];
        }
        else
        {
           n=0;
        }
        if(a!=0 && b!=0)
        {
        o=A[a-1][b-1];
        }
        else
        {
          o=0;
        }
      //  cout<<a<<" "<<b<<" "<<c<<" "<<d<<endl;
       // cout<<l<<" "<<m<<" "<<n<<" "<<o<<endl;
    
    
    f[i]=(l-m-n+o);
    if(f[i]<0)
    {
        f[i]+=1000000007;
    }
    else
    {
        f[i]%=1000000007;
    }
    
    
    
    }
    for(int i=0;i<B.size();i++)
    {
        if(f[i]<0)
        {
            f[i]+=1000000007;
        }
    }

return f;
}
```

### Mine (cpp, fails tests)
```cpp
vector<int> Solution::solve(vector<vector<int> > &mat, vector<int> &B, vector<int> &C,
			vector<int> &D, vector<int> &E) {
    int M = mat.size();
    int N = mat[0].size();
    int mod = 1e9+7;
    vector<vector<long long>> aux(M, vector<long long>(N));
    for (int i=0; i<N; i++) aux[0][i] = mat[0][i]; 
    for (int i=1; i<M; i++) for (int j=0; j<N; j++) aux[i][j] = (mat[i][j] + aux[i-1][j]) % mod; 
    for (int i=0; i<M; i++) for (int j=1; j<N; j++) aux[i][j] = (aux[i][j] + aux[i][j-1]) % mod;
    vector<int> out;
    for (int i=0; i<B.size(); i++) {
        int tli = B[i]-1, tlj = C[i]-1, rbi = D[i]-1, rbj = E[i]-1;
        long long res = aux[rbi][rbj]; 
        if (tli > 0) res = (res - aux[tli-1][rbj]) % mod; 
        if (tlj > 0) res = (res - aux[rbi][tlj-1]) % mod; 
        if (tli > 0 && tlj > 0) res = (res + aux[tli-1][tlj-1]) % mod;
        out.push_back(res);
    }
    return out;
}
```

### Mine (python, passes tests)
```python
def preProcess(mat, aux): 
    M = len(mat)
    N = len(mat[0])
    for i in range(0, N, 1): aux[0][i] = mat[0][i] 
    for i in range(1, M, 1):
        for j in range(0, N, 1): aux[i][j] = mat[i][j] + aux[i - 1][j]
    for i in range(0, M, 1):
        for j in range(1, N, 1): aux[i][j] += aux[i][j - 1] 

def sumQuery(aux, tli, tlj, rbi, rbj): 
    res = aux[rbi][rbj] 
    if (tli > 0): res -= aux[tli - 1][rbj] 
    if (tlj > 0):  res -= aux[rbi][tlj - 1] 
    if (tli > 0 and tlj > 0): res += aux[tli - 1][tlj - 1] 
    return res 

class Solution:
    def solve(self, A, B, C, D, E):
        aux = A
        preProcess(A, aux)
        for i in range(len(B)):
            yield sumQuery(aux, B[i]-1, C[i]-1, D[i]-1, E[i]-1) % int(1e9+7)
```
