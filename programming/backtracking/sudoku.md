# Sudoku

https://www.interviewbit.com/problems/sudoku/

Write a program to solve a Sudoku puzzle by filling the empty cells.
Empty cells are indicated by the character '.'
You may assume that there will be only one unique solution.

A sudoku puzzle:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

And its solution numbers marked in red:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

### Example

For the above given diagrams, the corresponding input to your program will be

```
[[53..7....], [6..195...], [.98....6.], [8...6...3], [4..8.3..1], [7...2...6], [.6....28.], [...419..5], [....8..79]]
```

and we would expect your program to modify the above array of array of characters to

```
[[534678912], [672195348], [198342567], [859761423], [426853791], [713924856], [961537284], [287419635], [345286179]]
```

## Hint 1

Think about the most naive solution.
You can try filling any available position with any valid character and check if sudoku now can be solved.

## Solution Approach

Classic backtrack problem. 
Everytime you place an element x on row,col, you need to check if its still valid to put x on that position
by double checking if x occurs more than once in the row or column or in its block.
If not, you proceed by placing x, and call forward to check if a correct solution
is possible with x in position row, col. 
If a solution is possible, you return the current configuration and you are done. Otherwise you try other values.

## Solution

```cpp
bool FindEmptyPos(vector<vector<char>> &A, int &row, int &col) {
    for (row = 0; row < 9; row++)
        for (col = 0; col < 9; col++)
            if (A[row][col] == '.')
                return true;

    return false;
}

bool isValid(vector<vector<char>> &A, int row, int col, int n) {
    char num = n + '0';
    for (int r = 0; r < 9; r++)
        if (A[r][col] == num) return false;

    for (int c = 0; c < 9; c++)
        if (A[row][c] == num) return false;

    int srow = row / 3, scol = col / 3;
    for (int r = srow * 3; r < (srow + 1) * 3; r++)
        for (int c = scol * 3; c < (scol + 1) * 3; c++)
            if (A[r][c] == num)
                return false;

    return true;
}

bool solver(vector<vector<char>> &A) {
    int row, col;
    if (FindEmptyPos(A, row, col) == false)
        return true;

    for (int n = 1; n <= 9; n++) {
        if (isValid(A, row, col, n)) {
            A[row][col] = n + '0';
            if (solver(A) == true)
                return true;

            A[row][col] = '.';
        }
    }
    return false;
}

void Solution::solveSudoku(vector<vector<char>> &A) {
    bool ans = solver(A);
}
```