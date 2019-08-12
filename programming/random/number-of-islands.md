# Number of islands

https://www.interviewbit.com/problems/number-of-islands/

Given a matrix of integers A of size N x M consisting of 0 and 1.
A group of connected 1’s forms an island. From a cell (i, j) such that A[i][j] = 1 
you can visit any cell that shares a corner with (i, j) and value in that cell is 1.

```
More formally, from any cell (i, j) if A[i][j] = 1 you can visit:

(i-1, j) if (i-1, j) is inside the matrix and A[i-1][j] = 1.

(i, j-1) if (i, j-1) is inside the matrix and A[i][j-1] = 1.

(i+1, j) if (i+1, j) is inside the matrix and A[i+1][j] = 1.

(i, j+1) if (i, j+1) is inside the matrix and A[i][j+1] = 1.

(i-1, j-1) if (i-1, j-1) is inside the matrix and A[i-1][j-1] = 1.

(i+1, j+1) if (i+1, j+1) is inside the matrix and A[i+1][j+1] = 1.

(i-1, j+1) if (i-1, j+1) is inside the matrix and A[i-1][j+1] = 1.

(i+1, j-1) if (i+1, j-1) is inside the matrix and A[i+1][j-1] = 1.
```

Return the number of islands.

### Note

Rows are numbered from top to bottom and columns are numbered from left to right.

Your solution will run on multiple test cases. If you are using global variables, make sure to clear them.

### Input Format

The only argument given is the integer matrix A.

### Output Format

Return the number of islands.


### Constraints
```
1 <= N, M <= 100
0 <= A[i] <= 1
```
### For Example
```
Input 1:
    A = [   [0, 1, 0]
            [0, 0, 1]
            [1, 0, 0]   ]
Output 1:
    2

Input 2:
    A = [   [1, 1, 0, 0, 0]
            [0, 1, 0, 0, 0]
            [1, 0, 0, 1, 1]
            [0, 0, 0, 0, 0]
            [1, 0, 1, 0, 1]    ]
Output 2:
    5
```

## Hint 1

The problem reduces to finding the number of connected components.

It can be solve with both BFS and DFS.

## Solution Approach

Whenever a cell with unvisited value ‘1’ is encountered we explore all the nodes that are reachable 
from it and continue exploring until no more nodes are left to explore.while exploring we mark them 
visited so that no nodes can be explored twice.

## Solution
### Editorial
```cpp
int dir[][2]={{0,1},{1,0},{-1,0},{0,-1},{1,-1},{-1,1},{1,1},{-1,-1}};
int tc=0;

int visited[1005][1005];

bool check(int i,int j,int n,int m,vector<vector<int> > &A){
    return (i>=0 && i<n && j>=0 && j<m && (A[i][j]==1) && visited[i][j]!=tc);
}

void dfs(int i,int j,int n,int m,vector<vector<int> > &A){
    visited[i][j]=tc;
    int di,dj;
    for(int k=0; k<8; ++k){
        di=i+dir[k][0];
        dj=j+dir[k][1];
        if(check(di,dj,n,m,A))
            dfs(di,dj,n,m,A);
        }
}

int solveit(vector<vector<int> > A){
    int n=A.size();
    int m=A[0].size();
    ++tc;
    int numberofislands=0;
    for(int i=0; i<n; ++i){
        for(int j=0; j<m; ++j){
                if(A[i][j]==1 && visited[i][j]!=tc){
                    dfs(i,j,n,m,A);
                    ++numberofislands;
                }
            }
        }
    return numberofislands;
}

int Solution::solve(vector<vector<int> > &A) {
    return solveit(A);
}
```

### Fastest
```cpp
int dir[][2]={{0,1},{1,0},{-1,0},{0,-1},{1,-1},{-1,1},{1,1},{-1,-1}};
int tc=0;

int visited[1005][1005];

bool check(int i,int j,int n,int m,vector<vector<int> > &A){
    return (i>=0 && i<n && j>=0 && j<m && (A[i][j]==1) && visited[i][j]!=tc);
}

void dfs(int i,int j,int n,int m,vector<vector<int> > &A){
    visited[i][j]=tc;
    int di,dj;
    for(int k=0; k<8; ++k){
        di=i+dir[k][0];
        dj=j+dir[k][1];
        if(check(di,dj,n,m,A))
            dfs(di,dj,n,m,A);
        }
}

int solveit(vector<vector<int> > A){
    int n=A.size();
    int m=A[0].size();
    assert(n>=1&&n<=100);
    assert(m>=1&&m<=100);
    ++tc;
    int numberofislands=0;
    for(int i=0; i<n; ++i){
        for(int j=0; j<m; ++j){
                if(A[i][j]==1 && visited[i][j]!=tc){
                    dfs(i,j,n,m,A);
                    ++numberofislands;
                }
            }
        }
    return numberofislands;
}

int Solution::solve(vector<vector<int> > &A) {
    return solveit(A);
}
```

### Lightweight
```cpp
void dfs(vector<vector<int> >&A, int i,int j,int n,int m)
{
    queue<pair<int,int> >q;
    q.push({i,j});
    pair<int,int>temp;
    A[i][j]=0;
    while(!q.empty())
    {
        temp=q.front();
        q.pop();
        i=temp.first;
        j=temp.second;
        if(i>0 && A[i-1][j]==1)
        {
            A[i-1][j]=0;
            q.push({i-1,j});
        }
        if(j>0 && A[i][j-1]==1)
        {
            A[i][j-1]=0;
            q.push({i,j-1});
        }
        if(j<m-1 && A[i][j+1]==1)
        {
            A[i][j+1]=0;
            q.push({i,j+1});
        }
        if(i<n-1 && A[i+1][j]==1)
        {
            A[i+1][j]=0;
            q.push({i+1,j});
        }
        if(i>0 && j>0 &&A[i-1][j-1]==1)
        {
            A[i-1][j-1]=0;
            q.push({i-1,j-1});
        }
        if(i>0 && j<m-1 && A[i-1][j+1]==1)
        {
            A[i-1][j+1]=0;
            q.push({i-1,j+1});
        }
        if(i<n-1 && j>0 && A[i+1][j-1]==1)
        {
            A[i+1][j-1]=0;
            q.push({i+1,j-1});
        }
        if(i<n-1 && j<m-1 && A[i+1][j+1]==1)
        {
            A[i+1][j+1]=0;
            q.push({i+1,j+1});
        }
        
    }
}
int Solution::solve(vector<vector<int> > &A) 
{
    int n=A.size(),m=A[0].size(),cnt=0;
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<m;j++)
        {
            if(A[i][j]==1)
            {
                A[i][j]=0;
               dfs(A,i,j,n,m);
               cnt++;
            }
        }
    }
    return cnt;
}
```

### Mine
```cpp
void floodfill_bfs(vector<vector<int>> & data, int i, int j) {
    queue<pair<int,int>> q;
    q.push(make_pair(i, j));
    while (!q.empty()) {
        auto p = q.front();
        q.pop();
        int i=p.first, j = p.second;
        if (i<0 || i>=data.size() || j<0 || j>=data[0].size() || data[i][j]!=1)
            continue;
        data[i][j] = 2;
        for (int dx=-1; dx<=1; dx++)
            for (int dy=-1; dy<=1; dy++)
                q.push(make_pair(i+dx, j+dy));
    }
}

int Solution::solve(vector<vector<int> > &data) {
    int res = 0;
    for (int i=0; i<data.size(); i++)
        for (int j=0; j<data[i].size(); j++)
            if (data[i][j]==1) {
                res++;
                floodfill_bfs(data, i, j);
            }
    return res;
}
```
