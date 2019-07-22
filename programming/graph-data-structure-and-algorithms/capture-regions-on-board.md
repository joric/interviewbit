# Capture Regions on Board

https://www.interviewbit.com/problems/capture-regions-on-board/

Given a 2D board containing 'X' and 'O', capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

For example,
```
X X X X
X O O X
X X O X
X O X X
```
After running your function, the board should be:
```
X X X X
X X X X
X X X X
X O X X
```

## Hint 1
Note that all the chunks of O which remain as O are the ones which have at least one O connected to them which is on the boundary. Otherwise they will turn into X.

Think of graph traversal.

## Solution Approach
We already know chunks of O which remain as O are the ones which have at least one O connected to them which is on the boundary.

Use BFS starting from 'O's on the boundary and mark them as 'B', then iterate over the whole board and mark 'O' as 'X' and 'B' as 'O'.

## Solution

### Editorial
```cpp
void bfsBoundary(vector<vector<char>> &board, int w, int l) {
    int width = board.size();
    int length = board[0].size();
    deque<pair<int, int>> q;
    q.push_back(make_pair(w, l));
    board[w][l] = 'B';
    while (!q.empty()) {
        pair<int, int> cur = q.front();
        q.pop_front();
        pair<int, int> adjs[4] = {
            { cur.first - 1, cur.second },
            { cur.first + 1, cur.second },
            { cur.first, cur.second - 1 },
            { cur.first, cur.second + 1 }
        };
        for (int i = 0; i < 4; ++i) {
            int adjW = adjs[i].first;
            int adjL = adjs[i].second;
            if ((adjW >= 0) && (adjW < width) && (adjL >= 0) && (adjL < length) && (board[adjW][adjL] == 'O')) {
                q.push_back(make_pair(adjW, adjL));
                board[adjW][adjL] = 'B';
            }
        }
    }
}

/*
* Use BFS starting from 'O's on the boundary and mark them as 'B', 
* then iterate over the whole board and mark 'O' as 'X' and 'B' as 'O'.
*/
void Solution::solve(vector<vector<char>> &board) {
    int width = board.size();
    if (width == 0)
        return;
    int length = board[0].size();
    if (length == 0)
        return;

    for (int i = 0; i < length; ++i) {
        if (board[0][i] == 'O')
            bfsBoundary(board, 0, i);

        if (board[width - 1][i] == 'O')
            bfsBoundary(board, width - 1, i);
    }

    for (int i = 0; i < width; ++i) {
        if (board[i][0] == 'O')
            bfsBoundary(board, i, 0);
        if (board[i][length - 1] == 'O')
            bfsBoundary(board, i, length - 1);
    }

    for (int i = 0; i < width; ++i) {
        for (int j = 0; j < length; ++j) {
            if (board[i][j] == 'O')
                board[i][j] = 'X';
            else if (board[i][j] == 'B')
                board[i][j] = 'O';
        }
    }
}

```

### Fastest
```cpp
bool valid(int i, int j, int r, int c) {
    return (i >= 0 && i < r && j >= 0 && j < c);
}

void dfs(int i, int j, int r, int c, vector<vector<char>> &A) {
    A[i][j] = '-';

    if (valid(i + 1, j, r, c) && A[i + 1][j] == 'O') {
        dfs(i + 1, j, r, c, A);
    }
    if (valid(i - 1, j, r, c) && A[i - 1][j] == 'O') {
        dfs(i - 1, j, r, c, A);
    }
    if (valid(i, j + 1, r, c) && A[i][j + 1] == 'O') {
        dfs(i, j + 1, r, c, A);
    }
    if (valid(i, j - 1, r, c) && A[i][j - 1] == 'O') {
        dfs(i, j - 1, r, c, A);
    }

    return;
}

void Solution::solve(vector<vector<char>> &A) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details

    int r = A.size();
    int c = A[0].size();

    for (int i = 0; i < c; i++) {
        if (A[0][i] == 'O')
            dfs(0, i, r, c, A);
        if (A[r - 1][i] == 'O')
            dfs(r - 1, i, r, c, A);
    }

    for (int i = 0; i < r; i++) {
        if (A[i][0] == 'O')
            dfs(i, 0, r, c, A);
        if (A[i][c - 1] == 'O')
            dfs(i, c - 1, r, c, A);
    }

    // cout<<A[r-2][c-3];

    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
            if (A[i][j] == '-') {
                A[i][j] = 'O';
            } else {
                A[i][j] = 'X';
            }
        }
    }

    return;
}
```

### Mine

```cpp
// https://www.interviewbit.com/problems/capture-regions-on-board/

void Solution::solve(vector<vector<char> > &A) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
    
    int rows = A.size();
    
    if(rows == 0){
        return;
    }
    
    int cols = A[0].size(); 
    
    vector<vector<bool> > check(rows, vector<bool>(cols, false));
    
    queue<pair<int, int> > q;
    
    for(int i = 0; i < cols; i++){
        if(A[0][i] == 'O'){
            check[0][i] = true;
            q.push(make_pair(0, i));
        }
        if(A[rows-1][i] == 'O'){
            check[rows-1][i] = true;
            q.push(make_pair(rows-1, i));
        }
    }
    
    for(int i = 0; i < rows; i++){
        if(A[i][0] == 'O'){
            check[i][0] = true;
            q.push(make_pair(i, 0));
        }
        if(A[i][cols-1] == 'O'){
            check[i][cols-1] = true;
            q.push(make_pair(i, cols-1));
        }
    }
    
    int i, j;
    
    while(!q.empty()){
        i = q.front().first;
        j = q.front().second;
        
        q.pop();
        
        if(i-1 > 0 && A[i-1][j] == 'O' && check[i-1][j] == false){
            check[i-1][j] = true; 
            q.push(make_pair(i-1, j));
            
        } 
        if(i+1 < rows-1 && A[i+1][j] == 'O' && check[i+1][j] == false){
            check[i+1][j] = true; 
            q.push(make_pair(i+1, j));
            
        } 
        if(j-1 > 0 && A[i][j-1] =='O' && check[i][j-1] == false){
            check[i][j-1] = true; 
            q.push(make_pair(i, j-1));
            
        } 
        if(j+1 < cols-1 && A[i][j+1] == 'O' && check[i][j+1] == false){
            check[i][j+1] = true; 
            q.push(make_pair(i, j+1));
            
        } 
    }
    
    for(int i = 0; i < rows; i++){
        for(int j = 0; j < cols; j++){
            if(A[i][j] == 'O' && !check[i][j]){
                A[i][j] = 'X';
            }
        }
    }
    
}

```

