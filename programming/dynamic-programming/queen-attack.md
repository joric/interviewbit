# Queen Attack

https://www.interviewbit.com/problems/queen-attack/

On a N * M chessboard, where rows are numbered from 1 to N and columns from 1 to M, there are queens at some cells. Return a N * M array A, where A[i][j] is number of queens that can attack cell (i, j). While calculating answer for cell (i, j), assume there is no queen at that cell.

### Notes

```
Queen is able to move any number of squares vertically, horizontally or diagonally on a chessboard. A queen cannot jump over another queen to attack a position.
You are given an array of N strings, each of size M. Each character is either a 1 or 0 denoting if there is a queen at that position or not, respectively.
Expected time complexity is worst case O(N*M).
For example,

Let chessboard be,
[0 1 0]
[1 0 0]
[0 0 1]

where a 1 denotes a queen at that position.

Cell (1, 1) is attacked by queens at (2, 1), (1,2) and (3,3).
Cell (1, 2) is attacked by queen at (2, 1). Note that while calculating this, we assume that there is no queen at (1, 2).
Cell (1, 3) is attacked by queens at (3, 3) and (1, 2).
and so on...

Finally, we return matrix

[3, 1, 2]
[1, 3, 3]
[2, 3, 0]
```

## Hint 1

If you actually traverse in all 8 directions for each cell, total complexity in the worst case will be O(N * M * (N+M)).
Can you store some data for cells in such a way that for finding an answer to the cell (i, j) you just have to look at its neighbours only?

## Solution

If you actually traverse in all 8 directions for each cell, total complexity in worst case will be O(N * M * (N+M)).
Can you store some data for cells in such a way that for finding answer to cell (i, j) you just have to look at its neighbours only.

We define f(i, j, k) as a number of queen attacks on the cell (i, j) from direction k. Eight directions can be given numbers 0 to 7.
Now, to see how many attacks are there on a cell (i, j), we go to its neighbour in direction k(say n_i, n_j). If the cell (n_i, n_j) has a queen, then there is just 1 attack. Else, number of attacks is f(n_i, n_j, k).

Can you formulate base cases?

We just have to take the sum of f(i, j, k) for all k=0 to 7 to find the answer for the position (i, j).
The total number of states is O(N * M * 8) and the transition is O(1), so total complexity is O(N * M).

### Editorial
```cpp
//dp array
vector<vector<int> > dp[8];

//checks if cell (i, j) is valid or not
bool valid(int i, int j, int n, int m){
    if(i < 0 or i >= n or j < 0 or j >= m)return false;
    return true;
}

//direction vectors
int dir1[8] = {-1, -1, 0, 1, 1, 1, 0, -1};
int dir2[8] = {0, 1, 1, 1, 0, -1, -1, -1};

//returns dp(i, j, k) as defined in hint
int rec(int i, int j, int k, vector<string> & A, int n, int m){

    //memoisation
    int &ret = dp[k][i][j];
    if(ret != -1)return ret;
    
    ret=0;
    
    //new positions
    int ni = i + dir1[k];
    int nj = j + dir2[k];
    
    //if valid
    if(valid(ni, nj, n, m)){
        if(A[ni][nj] == '1') ret++;
        else ret += rec(ni, nj, k, A, n, m);
    }
    
    return ret;
}

vector<vector<int> > Solution::queenAttack(vector<string> &A) {
    //init dp array
    int n = A.size(), m = A[0].size();
    for(int i = 0; i < 8; i++){
        dp[i].clear();
        dp[i].resize(n, vector<int>(m, -1));
    }

    vector< vector<int> > ret(n, vector<int>(m, 0));

    //calculate dp for all positions
    for(int k = 0; k < 8; k++)
        for(int i = 0; i < n; i++)
            for(int j = 0; j < m; j++){
                ret[i][j] += rec(i, j, k, A, n, m);
            }
    return ret;
}
```

### Fastest
```cpp
vector<vector<int> > Solution::queenAttack(vector<string> &A) {
    int n=A.size();
    int m = A[0].length();
    vector< vector<int> > ans(n, vector<int>(m,8));
    for(int i=0; i<m; i++ ){
        for(int j=0; j<n; j++){
                if(A[j][i]=='1') { 
                    ans[j][i]--;
                    break;
                }
                else
                    ans[j][i]--;
            }
        for(int j=n-1; j>=0; j--){
                if(A[j][i]=='1') { 
                    ans[j][i]--;
                    break;
                }
                else
                    ans[j][i]--;
            }
        for(int j=n-1,l=i+1; j>=0&&l<m; j--,l++){  
                 if(A[j][l]=='1') { 
                    ans[j][l]--;
                    break;
                }
                else
                    ans[j][l]--;
        }
        for(int r=0,c=i;c!=m-1 && c>=0 && r<n;c--,r++)
        {
             if(A[r][c]=='1') { 
                    ans[r][c]--;
                    break;
                }
                else
                    ans[r][c]--;
            
        }
        for(int r=0,c=i+1;r<n && c<m ; r++,c++)
        {
              if(A[r][c]=='1') { 
                    ans[r][c]--;
                    break;
                }
                else
                    ans[r][c]--;
        }
        for(int r=n-1,c=i;c!=m-1&&r>=0&&c>=0;r--,c--)
        {
                if(A[r][c]=='1') { 
                    ans[r][c]--;
                    break;
                }
                else
                    ans[r][c]--;
        }
    }
    
    for(int i=0; i<n; i++ ){
        for(int j=0; j<m; j++){
                if(A[i][j]=='1') { 
                    ans[i][j]--;
                    break;
                }
                else
                    ans[i][j]--;
            }
        for(int j=m-1; j>=0; j--){
                if(A[i][j]=='1') { 
                    ans[i][j]--;
                    break;
                }
                else
                    ans[i][j]--;
            }
        for(int r=i,c=0;c<m && r>=0 ;c++,r--)
        {
                if(A[r][c]=='1') { 
                    ans[r][c]--;
                    break;
                }
                else
                    ans[r][c]--;
            }   
        for(int r=i,c=m-1;c>=0 && r<n ;c--,r++)
{                if(A[r][c]=='1') { 
                    ans[r][c]--;
                    break;
                }
                else
                    ans[r][c]--;
            } 
        for(int r=i,c=0;r<n && c<m ;r++,c++)
        {
                if(A[r][c]=='1') { 
                    ans[r][c]--;
                    break;
                }
                else
                    ans[r][c]--;
        }
        for(int r=i,c=m-1;c>=0 && r>=0 ;r--,c-- )
        {
                          if(A[r][c]=='1') { 
                    ans[r][c]--;
                    break;
                }
                else
                    ans[r][c]--;
            
            
            
        }
    }
    

    return ans;
}
```

### Lightweight
```cpp
int row[]={0,-1,-1,-1,0,1,1,1};
    int col[]={1,1,0,-1,-1,-1,0,1};
    
int n,m;
void rec(vector<string> &A,vector<vector<int> > &mem,int i,int j,int k)
{   if(i>=n || i<0 || j>=m || j<0) return;
    
    if(A[i][j]=='1') {mem[i][j]++;return;}
    mem[i][j]++;
    rec(A,mem,i+row[k],j+col[k],k);
}
vector<vector<int> > Solution::queenAttack(vector<string> &A) {
    
     n=A.size();
     m=A[0].size();
    vector<vector<int> > mem(n,vector<int> (m,0));

for(int i=0;i<n;i++)
for(int j=0;j<m;j++)
for(int k=0;k<8;k++)
{
    if(A[i][j]=='1')
    {
        rec(A,mem,i+row[k],j+col[k],k);
    }
}
return mem;
}

```

