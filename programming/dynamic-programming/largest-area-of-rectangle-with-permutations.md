# Largest Area Of Rectangle With Permutations

https://www.interviewbit.com/problems/largest-area-of-rectangle-with-permutations

Given a binary grid i.e. a 2D grid only consisting of 0's and 1's, find the area of the largest rectangle inside the grid such that all the cells inside the chosen rectangle should have 1 in them. You are allowed to permutate the columns matrix i.e. you can arrange each of the column in any order in the final grid. Please follow the below example for more clarity.

Lets say we are given a binary grid of 3 * 3 size.
```
1 0 1
0 1 0
1 0 0
```
At present we can see that max rectangle satisfying the criteria mentioned in the problem is of 1 * 1 = 1 area i.e either of the 4 cells which contain 1 in it. Now since we are allowed to permutate the columns of the given matrix, we can take column 1 and column 3 and make them neighbours. One of the possible configuration of the grid can be:
```
1 1 0
0 0 1
1 0 0
```

Now In this grid, first column is column 1, second column is column 3 and third column is column 2 from the original given grid. Now, we can see that if we calculate the max area rectangle, we get max area as 1 * 2 = 2 which is bigger than the earlier case. Hence 2 will be the answer in this case.


## Hint 1

Brute force approach would be to generate each possible permutations of the columns and then check which one generates the rectangle with the maximum area. 
Time Complexity of this approach would be 2^m * T where m denotes the number of columns and T denotes the time required to get the maximum area for a given grid. Now as this method takes exponential time, we can't use it as a solution to our problem because of higher constraints on the values of n and m. 
Now you have got the basic idea. Can you improve it by thinking any polynomial time solution? 

## Solution approach

Let's try to think polynomial time approach.

Let's say for each index i.e. (i, j) pair, we store a value which corresponds to the number of consecutive cells having 1 as their value which are directly above that cell starting from the given cell itself. Lets store this value in an array called count. Thus count[i][j] will denote the number of consecutive 1's starting from the cell (i, j) and continuing upwards. 

For example, for a given matrix

```
1 0 1
1 1 0
1 0 1 
```
Its count matrix will be
```
1 0 1
2 1 0
3 0 1 
```
 At index i = 3 & j = 1, its value is 3 because there are 3 1's directly above it including itself. At index i = 2 & j = 3, its value is 0 as its value itself is 0, hence no consecutive 1's chain can be formed starting from this index.

We can use simple prefix sum method to evaluate the count array. 

Now, once we have got this array, let's consider a row i. Each element of this row will have some count[i][j] value where j is from 1 to m. Now as permutation is allowed, we can select any order for keeping the columns. Let's fix that ordering for time being with lower the value of count[i][j], the earlier it is getting placed. So we will have a sorted arrangement of columns according to their count[i][j] values. We can easily see that while traversing through the above sorted arrangement, we can calculate the maximum area possible for that particular row. On repeating this algorithm for each row of the grid will give us the maximum area rectangle possible in the given grid. 

The above method time complexity is O(n * m * LOG(m)) where n is the number of rows and m is the number of columns. 

This can be further improved by using count sort in place of normal sorting algorithms like quick sort. We can use count sort as maximum number of consecutive 1's in a column would be the size of the row i.e. n. On using count sort, We will get time complexity as O(n * m).

For more reference, please see the editorial solution.

## Solution

```cpp

// editorial

int Solution::solve(vector<vector<int>> &A) {
    int n, m, res = 0;
    n = A.size();
    m = A[0].size();
    int dp[n + 1][m + 1];
    memset(dp, 0, sizeof(dp));
    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (A[j - 1][i - 1] == 0) {
                dp[j][i] = 0;
            } else {
                dp[j][i] += dp[j - 1][i] + 1;
            }
        }
    }
    for (int i = 1; i <= n; ++i) {
        int arr[n + 1], cnt = 0;
        memset(arr, 0, sizeof(arr));
        for (int j = 1; j <= m; ++j) {
            arr[dp[i][j]]++;
        }
        for (int j = n; j >= 0; --j) {
            cnt += arr[j];
            res = max(res, cnt * j);
        }
    }
    return res;
}

///////////
// fastest

int Solution::solve(vector<vector<int>> &mat) {
    int R = mat.size();
    int C = mat[0].size();
    int hist[R + 1][C + 1];

    // Step 1: Fill the auxiliary array hist[][]
    for (int i = 0; i < C; i++) {
        // First row in hist[][] is copy of first row in mat[][]
        hist[0][i] = mat[0][i];

        // Fill remaining rows of hist[][]
        for (int j = 1; j < R; j++)
            hist[j][i] = (mat[j][i] == 0) ? 0 : hist[j - 1][i] + 1;
    }

    // Step 2: Sort rows of hist[][] in non-increasing order
    for (int i = 0; i < R; i++) {
        vector<int> count(R + 1, 0);

        // counting occurrence
        for (int j = 0; j < C; j++)
            count[hist[i][j]]++;

        //  Traverse the count array from right side
        int col_no = 0;
        for (int j = R; j >= 0; j--) {
            if (count[j] > 0) {
                for (int k = 0; k < count[j]; k++) {
                    hist[i][col_no] = j;
                    col_no++;
                }
            }
        }
    }

    // Step 3: Traverse the sorted hist[][] to find maximum area
    int curr_area, max_area = 0;
    for (int i = 0; i < R; i++) {
        for (int j = 0; j < C; j++) {
            // Since values are in decreasing order,
            // The area ending with cell (i, j) can
            // be obtained by multiplying column number
            // with value of hist[i][j]
            curr_area = (j + 1) * hist[i][j];
            if (curr_area > max_area)
                max_area = curr_area;
        }
    }
    return max_area;
}

//////////////

int Solution::solve(vector<vector<int>> &A) {

    int hist[A.size() + 1][A[0].size() + 1];

    for (int i = 0; i < A[0].size(); i++) {
        hist[0][i] = A[0][i];

        for (int j = 1; j < A.size(); j++)
            hist[j][i] = (A[j][i] == 0) ? 0 : hist[j - 1][i] + 1;
    }

    for (int i = 0; i < A.size(); i++) {
        int count[A.size() + 1];
        memset(count, 0, (A.size() + 1) * sizeof(int));

        for (int j = 0; j < A[0].size(); j++)
            count[hist[i][j]]++;

        int col_no = 0;
        for (int j = A.size(); j >= 0; j--) {
            if (count[j] > 0) {
                for (int k = 0; k < count[j]; k++) {
                    hist[i][col_no] = j;
                    col_no++;
                }
            }
        }
    }

    int curr_area, max_area = 0;
    for (int i = 0; i < A.size(); i++) {
        for (int j = 0; j < A[0].size(); j++) {
            curr_area = (j + 1) * hist[i][j];
            if (curr_area > max_area)
                max_area = curr_area;
        }
    }
    return max_area;
}
```