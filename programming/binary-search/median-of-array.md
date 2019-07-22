# Median Of Array

https://www.interviewbit.com/problems/median-of-array



There are two sorted arrays A and B of size m and n respectively.

Find the median of the two sorted arrays ( The median of the array formed by merging both the arrays ).

The overall run time complexity should be O(log (m+n)).

Sample Input

A: [1 4 5]
B: [2 3]

Sample Output

3

NOTE: IF the number of elements in the merged array is even, then the median is the
average of n / 2 th and n/2 + 1th element. 
For example, if the array is [1 2 3 4], the median is (2 + 3) / 2.0 = 2.5 

## Solution

```cpp

/* editorial */

double Solution::findMedianSortedArrays(const vector<int> &A, const vector<int> &B) {
    int m = A.size(), n = B.size();
    if (m > n)
        return findMedianSortedArrays(B, A);
    int i, j, i_min = 0, i_max = m;
    int med1, med2;
    while (i_min <= i_max) {
        i = (i_max + i_min) / 2;
        j = (m + n + 1) / 2 - i;
        if (j > 0 && i < m && B[j - 1] > A[i])
            i_min = i + 1;
        else if (i > 0 && j < n && A[i - 1] > B[j])
            i_max = i - 1;
        else {
            if (i == 0) {
                //median numbers will be array B
                med1 = B[j - 1];
            } else if (j == 0) {
                //median number will be in array A
                med1 = A[i - 1];
            } else {
                med1 = max(A[i - 1], B[j - 1]);
            }
            if ((m + n) % 2 == 1)
                return med1;
            //else even numbers
            //find med2
            if (i == m)
                med2 = B[j];
            else if (j == n)
                med2 = A[i];
            else
                med2 = min(A[i], B[j]);
            //cout<<med1<<" "<<med2<<endl;
            return (1.0 * (med1 + med2)) / 2;
        }
    }
}

/* my solution */

double Solution::findMedianSortedArrays(const vector<int> &A, const vector<int> &B) {
    int n = A.size(), m = B.size();

    int lo = -1, hi = n - 1;      // all values of i
    int even = ((n + m) & 1) ^ 1; // is it odd case median.

    while (lo <= hi) {
        int mid = (lo + hi) / 2; // last element taken from A

        int r = n - mid - 1;          // total element left from A
        int needed = (n + m) / 2 - r; // all element needed from B to complement.
        // taken too much from the right of A

        // printf("taken up to %d with r %d and needs %d\n", mid, r, needed);
        if (needed < 0) {
            lo = mid + 1;
            continue;
        }
        // taken too little from the right of A
        if (needed > m) {
            hi = mid - 1;
            continue;
        }

        int i = mid;
        int j = m - needed - 1; // index of last element in B

        int left_max = i > -1 ? A[i] : INT_MIN;
        if (j >= 0)
            left_max = max(left_max, B[j]);

        int right_min = min(j + 1 >= m ? INT_MAX : B[j + 1], i + 1 >= n ? INT_MAX : A[i + 1]);
        // printf("%d %d\n", left_max, right_min);
        if (left_max <= right_min) {
            double median = left_max;
            if (even) {
                median += right_min;
                return median / 2;
            } else
                return median;
        } else {
            // we have to reupdate our indexeses.
            if (i > -1 && left_max == A[i]) {
                hi = mid - 1;
            } else {
                lo = mid + 1;
            }
        }
    }

    return -1;
}
```