# Set Matrix Zeros

https://www.interviewbit.com/problems/set-matrix-zeros/

Given an m x n matrix of 0s and 1s, if an element is 0, set its entire row and column to 0.

Do it in place.

Example

Given array A as
```
1 0 1
1 1 1 
1 1 1
```
On returning, the array A should be :
```
0 0 0
1 0 1
1 0 1
```
Note that this will be evaluated on the extra memory used. Try to minimize the space and time complexity.


## Solution

```cpp
void Solution::setZeroes (vector < vector < int >>&grid) {
	int rows = grid.size ();
	int cols = grid[0].size ();

	// Check if row 0 has a 0. Save this info for later.
	bool row0Has0 = false;
	for (int col = 0; col < cols; col++) {
		if (grid[0][col] == 0) {
			row0Has0 = true;
		}
	}

	// Check if col 0 has a 0. Save this info for later.
	bool col0Has0 = false;
	for (int row = 0; row < rows; row++) {
		if (grid[row][0] == 0) {
			col0Has0 = true;
		}
	}

	// Use row 0 and col 0 as storage.
	// Loop through grid and save which rows and columns have 0s.
	for (int row = 0; row < rows; row++) {
		for (int col = 0; col < cols; col++) {
			if (grid[row][col] == 0) {
				grid[row][0] = 0;
				grid[0][col] = 0;
			}
		}
	}

	// Zero out the necessary cells in grid (except for 0th row and 0th col).
	for (int row = 1; row < rows; row++) {
		for (int col = 1; col < cols; col++) {
			if (grid[row][0] == 0 || grid[0][col] == 0) {
				grid[row][col] = 0;
			}
		}
	}

	// Zero out the necessary cells in row 0.
	if (row0Has0) {
		for (int col = 0; col < cols; col++) {
			grid[0][col] = 0;
		}
	}

	// Zero out the necessary cells in col 0.
	if (col0Has0) {
		for (int row = 0; row < rows; row++) {
			grid[row][0] = 0;
		}
	}
}
