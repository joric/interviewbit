# Word Search Board

https://www.interviewbit.com/problems/word-search-board/

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The cell itself does not count as an adjacent cell. 
The same letter cell may be used more than once.

Example :

Given board =
```
[
  ["ABCE"],
  ["SFCS"],
  ["ADEE"]
]
```
```
word = "ABCCED", -> returns 1,
word = "SEE", -> returns 1,
word = "ABCB", -> returns 1,
word = "ABFSAB" -> returns 1
word = "ABCD" -> returns 0
```
Note that 1 corresponds to true, and 0 corresponds to false.

## Hint 1
Think recursively.

Say the given word is [1...N]. If you can match [2...N] starting at every position of board how it can help you to compute the current answers for every position.

## Solution Approach


Lets look at the bruteforce approach. 

You iterate over every cell of the matrix to explore if it could be the starting point. Then for every neighboring character which has the same character as the next character in the string, we explore if rest of the string can be formed using that neighbor cell as the starting point.

To sum it up
```
exist(board, word, i , j) is true if for any neighbor (k,l) of (i,j) 
exist(board, word[1:], k, l) is true
```

Now note that we could memoize the answer for exist(board, word suffix length, i, j).


## Solution

### Editorial
```cpp
bool atleastOneNeighborTrue(vector<vector<int>> &doesExist, int r, int c) {
    if (r > 0 && doesExist[r - 1][c]) return true;
    if (c > 0 && doesExist[r][c - 1]) return true;
    if (r + 1 < doesExist.size() && doesExist[r + 1][c]) return true;
    if (c + 1 < doesExist[0].size() && doesExist[r][c + 1]) return true;
    return false;
}

int Solution::exist(vector<string> &board, string word) {
    if (0 == word.length()) {
        return 1;
    }

    int row = board.size();
    if (row == 0) return 0;

    int col = board[0].size();
    if (col == 0) return 0;

    vector<vector<int>> doesExist[2];
    for (int j = 0; j < row; j++) {
        vector<int> temp(col, 0), temp2(col, 0);
        doesExist[0].push_back(temp);
        doesExist[1].push_back(temp2);
    }

    int cur = 0;

    for (int i = 0; i < word.length(); i++) {
        for (int j = 0; j < row; j++) {
            for (int k = 0; k < col; k++) {
                if (i == 0) {
                    doesExist[cur][j][k] = (word[i] == board[j][k]);
                    continue;
                }
                doesExist[cur][j][k] = (word[i] == board[j][k] && atleastOneNeighborTrue(doesExist[1 - cur], j, k));
            }
        }
        cur = 1 - cur;
    }
    cur = 1 - cur;
    for (int j = 0; j < row; j++) {
        for (int k = 0; k < col; k++) {
            if (doesExist[cur][j][k]) return 1;
        }
    }
    return 0;
}
```

### Fastest
```cpp
bool isExist(int i, int j, int index, vector<string> &A, string B) {
    if(index == B.length())
        return true;
        
    int n = A.size();
    int m = A[0].size();
    
    if(i < 0 || j < 0 || i >= n || j >= m)
        return false;
    
    if(A[i][j] != B[index])
        return false;
    
    return isExist(i, j + 1, index+1, A, B) ||
           isExist(i, j - 1, index+1, A, B) ||
           isExist(i + 1, j, index+1, A, B) ||
           isExist(i - 1, j, index+1, A, B);
}

int Solution::exist(vector<string> &A, string B) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
    int b = B.length();
    if(b == 0) return 1;
        
    int n = A.size();
    if(n == 0) return 0;
    
    int m = A[0].size();
    if(m == 0) return 0;
    
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < m; j++) {
            if(A[i][j] == B[0]) {
                bool temp = isExist(i, j, 0, A, B);
                if(temp)
                    return 1;
            }
        }
    }
    
    return 0;
}


```

### Mine

```cpp
vector<int> x({0, 1, -1, 0});
vector<int> y({1, 0, 0, -1});
 
// Check if the coordinates are safe to visit
bool isSafe(int a, int b, int c, int d){
    if(a >= 0 && a < c && b >= 0 && b < d)return true;
    return false;
}


void explore(bool& pivot, int val, int a, int b, vector<string>& A, string B){
    // If reached on the last index of the string,
    // it means we have found the reqd. string and 
    // so we return
    if(val == B.size()-1){
        pivot = true;
        return;
    }

    for(int i = 0; i < x.size(); i++){
        int first = a + x[i];
        int second = b + y[i];
        
        // Explore the adjacent node only if its value is the next index in the
        // given string
        if(isSafe(first, second, A.size(), A[0].size()) && A[first][second] == B[val+1]){
            explore(pivot, val+1, first, second, A, B);
            // To reduce time limit.
            if(pivot == true)return;
        }
    }

}

int Solution::exist(vector<string> &A, string B) {
    int l = A.size(), m = A[0].size();
    if(l == 0)return 0;

    bool pivot = false;
    for(int i = 0; i < l; i++){
        for(int k = 0; k < m; k++){
            if(A[i][k] == B[0]){
                explore(pivot, 0, i, k, A, B);
            }
            if(pivot)return pivot;
        }
    }
    return pivot;
}

```

## Asked in
* Epic systems
* Amazon

