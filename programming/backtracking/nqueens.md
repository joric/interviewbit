# Nq Queens

https://www.interviewbit.com/problems/nq-queens

The n-queens puzzle is the problem of placing n queens on an n?n chessboard such that no two queens attack each other.

### N Queens: Example 1

Given an integer n, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.

### For example,
There exist two distinct solutions to the 4-queens puzzle:

```
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

Unfortunately, there is no magic trick to solve this problem. This is more of a bruteforce problem. A more intelligent bruteforce.

## Hint 1

Think recursively. Store which places are occupied already and move on to next row and fill in either of the available positions and go on.

### Notes:

1. There can exactly be one queen per row. Otherwise the 2 queens in the row would collide. If you miss out on a row, there cannot be N queens on the board. 
2. Every column needs to have exactly one queen. 
3. The left diagonal cannot have more than one queen ( Unique (row + col) )
4. The right diagonal cannot have more than one queen ( Unique (row - col) )

We can start placing a queen per row. When placing a queen on a row, col, we need to check if the position is available based on what we have already placed. Then we move to the next row.

## Solution

```cpp

// editorial

class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> solutions;
        vector<int> solution(n);
        solveNQueensImpl(0, solution, solutions);
        return solutions;
    }

    void solveNQueensImpl(int row, vector<int> &solution, vector<vector<string>> &solutions) {
        int n = solution.size();
        if (row == n) {
            solutions.push_back(solToStrings(solution));
            return;
        }
        // For each column...
        for (int j = 0; j < n; ++j) {
            // Skip if there is another queen in this column or diagonals
            if (isAvailable(solution, row, j)) {
                solution[row] = j;
                solveNQueensImpl(row + 1, solution, solutions);
            }
        }
    }

    bool isAvailable(const vector<int> &solution, int i, int j) {
        for (int k = 0; k < i; ++k) {
            if (j == solution[k] || i + j == k + solution[k] || i - j == k - solution[k])
                return false;
        }
        return true;
    }

    vector<string> solToStrings(const vector<int> &sol) {
        int n = sol.size();
        vector<string> sol_strings(n);
        for (int i = 0; i < n; ++i) {
            sol_strings[i] = string(n, '.');
            sol_strings[i][sol[i]] = 'Q';
        }
        return sol_strings;
    }
};

/////////////////////////

vector<vector<string>> ans;
int n;

void recur(int ind, vector<int> pos) {
    int i, j;
    if (ind == n) {
        vector<string> valid;
        for (i = 0; i < n; i++) {
            string row = "";
            for (j = 0; j < n; j++) {
                if (j == pos[i])
                    row += 'Q';
                else
                    row += '.';
            }
            valid.push_back(row);
        }
        ans.push_back(valid);

        return;
    }
    for (i = 0; i < n; i++) {
        bool flag = true;
        for (j = 0; j < pos.size(); j++) {
            if (abs(ind - j) == abs(pos[j] - i) || i == pos[j])
                flag = false;
        }
        if (!flag)
            continue;
        pos.push_back(i);
        recur(ind + 1, pos);
        pos.pop_back();
        //pos[ind] = -1;
    }
}
vector<vector<string>> Solution::solveNQueens(int A) {
    ans.clear();
    vector<int> pos;
    int i;
    n = A;
    recur(0, pos);
    return ans;
}

//////////////////////////////////////////////////////////////////

vector<vector<string>> ans;
int n;

void recur(int ind, vector<int> pos) {
    int i, j;
    if (ind == n) {
        vector<string> valid;
        for (i = 0; i < n; i++) {
            string row = "";
            for (j = 0; j < n; j++) {
                if (j == pos[i])
                    row += 'Q';
                else
                    row += '.';
            }
            valid.push_back(row);
        }
        ans.push_back(valid);

        return;
    }
    for (i = 0; i < n; i++) {
        bool flag = true;
        for (j = 0; j < pos.size(); j++) {
            if (abs(ind - j) == abs(pos[j] - i) || i == pos[j])
                flag = false;
        }
        if (!flag)
            continue;
        pos.push_back(i);
        recur(ind + 1, pos);
        pos.pop_back();
        //pos[ind] = -1;
    }
}
vector<vector<string>> Solution::solveNQueens(int A) {
    ans.clear();
    vector<int> pos;
    int i;
    n = A;
    recur(0, pos);
    return ans;
}
```