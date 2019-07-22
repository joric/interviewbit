# Valid Sudoku

https://www.interviewbit.com/problems/valid-sudoku



Very simple simulation problem. Just need to keep track of the digits seen in every row,
every column and every block as defined in the rules. 
Whenever you encounter a digit already seen, you know the sudoku is not valid.

Note that this problem will get very complicated if you were to determine if the sudoku was solvable.



## Solution

```cpp

#define GRIDSIZE 9

bool isValid(const vector<string> &A, int row, int col) {
    char elem = A[row][col];

    //check range
    if (elem - '0' < 1 || elem - '0' > 9)
        return false;

    //check for duplicates in row and col
    for (auto p = 0; p < GRIDSIZE; ++p) {
        if (A[p][col] == elem && p != row) return false;
        if (A[row][p] == elem && p != col) return false;
    }

    //check for duplicates in subgrids
    int subRow = (row / 3) * 3;
    int subCol = (col / 3) * 3;

    for (auto i = subRow; i < subRow + 3; ++i) {
        for (auto j = subCol; j < subCol + 3; ++j)
            if (A[i][j] == elem && (i != row || j != col))
                return false;
    }

    return 1;
}

int sudokuRunner(const vector<string> &A, int row, int col) {
    if (row == 9)
        return 1;

    int nextRow, nextCol;
    if (col == 8) {
        nextRow = row + 1;
        nextCol = 0;
    } else {
        nextRow = row;
        nextCol = col + 1;
    }

    if (A[row][col] != '.') {
        if (!isValid(A, row, col))
            return 0;
    }

    return sudokuRunner(A, nextRow, nextCol);
}

int Solution::isValidSudoku(const vector<string> &A) {
    if (A.size() != GRIDSIZE || A[0].size() != GRIDSIZE)
        return 0;
    return sudokuRunner(A, 0, 0);
}
```