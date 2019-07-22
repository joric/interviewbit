# Matrix Median

https://www.interviewbit.com/problems/matrix-median



Given a N cross M matrix in which each row is sorted, find the overall median of the matrix. Assume N*M is odd.

We cannot use extra memory, so we can't actually store all elements in an array and sort the array.
But since, rows are sorted it must be of some use, right?

Note that in a row you can binary search to find how many elements are smaller than a value X in O(log M).
This is the base of our solution.

Say k = N*M/2. We need to find (k + 1)^th smallest element.
We can use binary search on answer. In O(N log M), we can count how many elements are smaller
than X in the matrix.

So, we use binary search on interval [1, INT_MAX]. So, total complexity is O(30 * N log M).

Note:
This problem can be solve by using min-heap, but extra memory is not allowed.

## Solution

```cpp


int Solution::findMedian(vector<vector<int> > &A) {
    int min = A[0][0], max = A[0][0];
    int n = A.size(), m = A[0].size();
    for (int i = 0; i < n; ++i) {
        if (A[i][0] < min) min = A[i][0];
        if (A[i][m-1] > max) max = A[i][m-1];
    }

    int element = (n * m + 1) / 2;
    while (min < max) {
        int mid = min + (max - min) / 2;
        int cnt = 0;
        for (int i = 0; i < n; ++i)
            cnt += upper_bound(&A[i][0], &A[i][m], mid) - &A[i][0];
        if (cnt < element)
            min = mid + 1;
        else
            max = mid;
    }
    return min;
}

```