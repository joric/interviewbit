# Rotten Oranges

https://www.interviewbit.com/problems/rotten-oranges

Given a matrix of integers A of size N x M consisting of 0, 1 or 2.

Each cell can have three values:

* The value 0 representing an empty cell.
* The value 1 representing a fresh orange.
* The value 2 representing a rotten orange.

Every minute, any fresh orange that is adjacent (Left, Right, Top, or Bottom) to a rotten orange becomes rotten.
Return the minimum number of minutes that must elapse until no cell has a fresh orange.
If this is impossible, return -1 instead.

Note:

Your solution will run on multiple test cases. If you are using global variables, make sure to clear them.

### Input Format

The first argument given is the integer matrix A.

### Output Format

Return the minimum number of minutes that must elapse until no cell has a fresh orange.  
If this is impossible, return -1 instead.

### Constraints

* 1 <= N, M <= 1000
* 0 <= A[i] <= 2

### For Example
```
Input 1:
    A = [   [2, 1, 1]
            [1, 1, 0]
            [0, 1, 1]   ]
Output 1:
    4

Input 2:
    A = [   [2, 1, 1]
            [0, 1, 1]
            [1, 0, 1]   ]
Output 2:
    -1
```
## Solution Approach

Every turn, the rotting spreads from each rotting orange to other adjacent oranges.
Initially, the rotten oranges have ‘depth’ 0, and every time they rot a neighbor,
the neighbors have 1 more depth. We want to know the largest possible depth.

Use multi-source BFS to achieve this with all cells containing rotten oranges as starting nodes.
At the end check if there are fresh oranges left or not.

## Solution

### Editorial
```cpp
int Solution::solve(vector<vector<int> > &mat) {
    int r=mat.size();
    int c=mat[0].size();
    queue<pair<int,int> > q;
    for(int i=0; i<r; i++){
        for(int j=0;j<c;j++){
            if(mat[i][j] == 2){
                q.push(make_pair(i,j));
            }
        }
    }
    q.push(make_pair(-1,-1));
    int count=0;
    while(!q.empty()){
        pair<int,int> temp=q.front();
        q.pop();
        if(q.empty()){
            break;
        }
        if(temp.first==-1 && temp.second==-1){
            count++;
            q.push(temp);
        }
        else{
            if(temp.first-1>=0 && mat[temp.first-1][temp.second]==1){
                mat[temp.first-1][temp.second]=2;
                q.push(make_pair(temp.first-1,temp.second));
            }
            if(temp.first+1<r && mat[temp.first+1][temp.second]==1){
                mat[temp.first+1][temp.second]=2;
                q.push(make_pair(temp.first+1,temp.second));
            }
            if(temp.second-1>=0 && mat[temp.first][temp.second-1]==1){
                mat[temp.first][temp.second-1]=2;
                q.push(make_pair(temp.first,temp.second-1));
            }
            if(temp.second+1<c && mat[temp.first][temp.second+1]==1){
                mat[temp.first][temp.second+1]=2;
                q.push(make_pair(temp.first,temp.second+1));
            }
        }
    }
    bool flag=true;
    for(int i=0; i<r; i++){
        for(int j=0; j<c; j++){
            if(mat[i][j]==1){
                flag=false;
                break;
            }
        }
    if(!flag)   break;
    }
    if(flag){
        return count;
    }
    else{
        return -1;
    }
}
```

### Fastest
```cpp
int Solution::solve(vector<vector<int> > &A) {
    queue<pair<int,int> > q;
    int ans=0;
    for(int i=0;i<A.size();i++){
        for(int j=0;j<A[0].size();j++){
            if(A[i][j]==2){
                pair<int,int> p = make_pair(i,j);
                q.push(p);
            }
        }
    }
    
    if(q.empty()){
        return -1;
    }
    else{
        pair<int,int> p=make_pair(-1,-1);
        q.push(p);    
    }
    
    while(!q.empty()){
        pair <int,int> p= q.front();
        q.pop();
        int i=p.first;
        int j=p.second;
        if(i==-1&&j==-1){
            if(!q.empty()){
                ans++;
                pair<int,int> p= make_pair(-1,-1);
                q.push(p); 
            }
        }
        else{
            bool check=false;
            if(i-1>=0&&i-1<A.size()){
                if(A[i-1][j]==1){
                    A[i-1][j]=2;
                    pair<int,int> p = make_pair(i-1,j);
                    q.push(p);
                }
            }
            
            if(i+1>=0&&i+1<A.size()){
                if(A[i+1][j]==1){
                    A[i+1][j]=2;
                    pair<int,int> p = make_pair(i+1,j);
                    q.push(p);
                }
            }
            
            if(j-1>=0&&j-1<A[0].size()){
                if(A[i][j-1]==1){
                    A[i][j-1]=2;
                    pair<int,int> p = make_pair(i,j-1);
                    q.push(p);
                }
            }
            
            if(j+1>=0&&j+1<A[0].size()){
                if(A[i][j+1]==1){
                    A[i][j+1]=2;
                    pair<int,int> p = make_pair(i,j+1);
                    q.push(p);
                }
            }
            
        }
        
    }
    
    for(int i=0;i<A.size();i++){
        for(int j=0;j<A[0].size();j++){
            if(A[i][j]==1){
                return -1;
            }
        }
    }
    return ans;
}```

### Lightweight
```cpp
int Solution::solve(vector<vector<int> > &A) {
    int ma = INT_MIN, m, till=5, flag=0;
    for(int k=0; k<till; k++){
    for(int i=0; i<A.size(); i++)
    {
        for(int j=0; j<A[i].size(); j++)
        {
            m = INT_MAX;
            if(A[i][j]==1 || A[i][j]>2)
            {
                if(i+1<A.size() && A[i+1][j]>1)
                    m = min(m, A[i+1][j]);
                if(j-1>=0 && A[i][j-1]>1)
                    m = min(m, A[i][j-1]);
                if(i-1>=0 && A[i-1][j]>1)
                    m = min(m, A[i-1][j]);
                if(j+1<A[i].size() && A[i][j+1]>1)
                    m = min(m, A[i][j+1]);
                if(m!=INT_MAX)
                    A[i][j] = m+1;
                if(k==till-1)
                    ma = max(ma, A[i][j]);
            }
            if(k==till-1 && A[i][j]==1)
                return -1;
            // cout<<A[i][j]<<" ";
        }
        // cout<<endl;
    }}
    return ma-2;
}
```

### Mine
```python
from collections import deque
class Solution:
    def solve(self, A):
        q,n,m,c,res = deque(),len(A),len(A[0]),0,-1

        def neighbours(r, c):
            for nr, nc in ((r-1,c),(r,c-1), (r+1, c), (r, c+1)):
                if 0 <= nr < n and 0 <= nc < m:
                    yield nr, nc

        for i in range(n):
            for j in range(m):
                if A[i][j]==2: q.append((i,j))
                elif A[i][j]==1: c += 1

        while len(q):
            size = len(q)
            for _ in range(size):
                cx, cy = q.popleft()
                for x,y in neighbours(cx, cy):
                    if A[x][y] == 1:
                        A[x][y] = 2
                        c -= 1
                        q.append((x, y))
            res += 1

        return res if c==0 else -1
```

## References
* https://www.geeksforgeeks.org/minimum-time-required-so-that-all-oranges-become-rotten/
