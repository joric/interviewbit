# Black Shapes

https://www.interviewbit.com/problems/black-shapes/

```
Given N * M field of O's and X's, where O=white, X=black
Return the number of black shapes. A black shape consists of one or more adjacent X's (diagonals not included)

Example:

OOOXOOO
OOXXOXO
OXOOOXO

answer is 3 shapes are:
(i)
  X
X X

(ii)
X

(iii)
X
X

Note that we are looking for connected shapes here.

For example,

XXX
XXX
XXX

is just one single connected black shape.
```
## Hint 1

You need to find number of different connected components here. Any graph traversal algorithm can do this.

## Solution Approach

Simple graph traversal approach:
```
Answer := 0
Loop i = 1 to N :
    Loop j = 1 to M:
          IF MATRIX at i, j equal to 'X' and not visited:
                 BFS/DFS to mark the connected area as visited
                 update Answer
    EndLoop
EndLoop

return Answer
```

## Solution

### Editorial
```cpp
int dx[4] = {1, -1, 0, 0};
int dy[4] = {0, 0, 1, -1};
int N;
int M;
bool is_valid(int x, int y) {
    if(x < 0 || x >= N || y < 0 || y >= M)
            return false;
    return true;
}

void bfs(int i, int j, vector<string> &Vec) {
    queue<pair<int, int> > Que;
    Que.push(make_pair(i, j));

    while(!Que.empty()) {
        pair<int, int> P = Que.front();
        Que.pop();
        Vec[P.first][P.second] = 'O';
        for(int i = 0; i < 4; ++i) {
            int x = P.first + dx[i];
            int y = P.second + dy[i];
            if(is_valid(x, y) && Vec[x][y] == 'X') {
                Que.push(make_pair(x, y));
            }
        }
    }    
}

int Solution::black(vector<string> &Vec) {
    N = Vec.size();
    M = Vec[0].size();
    int cnt = 0;
    for(int i = 0; i < N; ++i) {
        for(int j = 0; j < M; ++j) {
            if(Vec[i][j] == 'X') {
                cnt++;
                bfs(i, j, Vec);
            }
        }
    }
    return cnt;
}
```
### Lightweight

```cpp
int dx[] = {1,0,-1, 0};
int dy[] = {0, 1, 0, -1};

void dfs(int r, int c, vector<string> &A){
    if(A[r][c] != 'X') return;
    A[r][c] = 'O';
    for(int i = 0; i < 4; i++){
        if(r+dx[i] >= 0 && r+dx[i] < A.size() 
        && c+dy[i] >= 0 && c+dy[i] < A[0].size()){
            dfs(r+dx[i] , c+dy[i], A);
        }
    }
    
}
int Solution::black(vector<string> &A) {
    //vector<vector<int> A[0].size()> vis(A.size(), false);
    int cnt = 0;
    for(int i = 0; i < A.size(); i++){
        for(int j = 0; j < A[0].size(); j++){
            if(A[i][j] == 'X'){
                dfs(i, j, A);
                cnt++;
            }
        }
    }
    return cnt;
}
```

### Mine

```cpp
// https://www.interviewbit.com/problems/black-shapes/ 

void dfs(vector<string> A, int i, int j, vector<vector<bool> >& visited, vector<vector<int> >& check){
    if(i < 0 || i >= A.size()){
        return;
    }
    if(j < 0 || j >= A[0].size()){
        return;
    }
    if(check[i][j] == 0 || visited[i][j]){
        return;
    }
    
    visited[i][j] = true;
    
    dfs(A, i-1, j, visited, check);
    dfs(A, i+1, j, visited, check);
    dfs(A, i, j-1, visited, check);
    dfs(A, i, j+1, visited, check);
}

int Solution::black(vector<string> &A) {
    // Do not write main() function.
    //Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
    
    if(A.size() == 0){
        return 0;
    }
    
    vector<vector<int> > check(A.size(), vector<int> (A[0].size(), 0));
    vector<vector<bool> > visited(A.size(), vector<bool> (A[0].size(), false));
    
    for(int i = 0; i < A.size(); i++){
        for(int j = 0; j < A[0].size(); j++){
            if(A[i][j] == 'X'){
                check[i][j] = 1;
            }
        }
    }
    
    
    int sol = 0;
    
    for(int i = 0; i < A.size(); i++){
        for(int j = 0; j < A[i].size(); j++){
            if(A[i][j] == 'X' && !visited[i][j]){
                dfs(A, i, j, visited, check);
                sol++;
            }
        }
    }
    
    return sol;
}

```

## Asked in
* Amazon
