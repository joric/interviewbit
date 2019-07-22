# Array 3 Pointers

https://www.interviewbit.com/problems/array-3-pointers


## Hint 1

The bruteforce solution is to pick one element each from a, b and c in a loop. O(N^3). 
Obviously something better is required in this case .

A better approach might be to:

Iterate over all elements of a,
Binary search for element just smaller than or equal to in b and c, and note the difference.
Repeat the process for b and c. O(n log n).
*Note: As we move over the element in a, the elements in b and c that we are trying to find will also move forward. *

Solution approach

Windowing strategy works here. 
Lets say the arrays are A,B and C.

Take 3 pointers X, Y and Z
Initialize them to point to the start of arrays A, B and C
Find min of X, Y and Z.
Compute max(X, Y, Z) - min(X, Y, Z).
If new result is less than current result, change it to the new result.
Increment the pointer of the array which contains the minimum.
Note that we increment the pointer of the array which has the minimum,
because our goal is to decrease the difference. Increasing the maximum pointer
is definitely going to increase the difference. Increase the second maximum pointer can potentially
increase the difference ( however, it certainly will not decrease the difference ).
## Solution

```cpp

// editorial

int Solution::minimize(const vector<int> &a, const vector<int> &b, const vector<int> &c) {
    int diff = INT_MAX, minimum = INT_MAX, maximum = INT_MIN;
    for (int i = 0, j = 0, k = 0; i < a.size() && j < b.size() && k < c.size();) {
        minimum = min(a[i], min(b[j], c[k]));
        maximum = max(a[i], max(b[j], c[k]));
        diff = min(diff, maximum - minimum);
        if (diff == 0) break;
        if (a[i] == minimum)
            i++;
        else if (b[j] == minimum)
            j++;
        else
            k++;
    }
    return diff;
}

int Solution::minimize(const vector<int> &A, const vector<int> &B, const vector<int> &C) {
    int result = INT_MAX;
    int temp1, temp2, temp3;
    int i = 0, j = 0, k = 0;

    while (i < A.size() && j < B.size() && k < C.size()) {
        temp1 = A[i] - B[j];
        temp2 = B[j] - C[k];
        temp3 = C[k] - A[i];
        result = min(result, max(abs(temp1), max(abs(temp2), abs(temp3))));
        if (abs(temp1) >= abs(temp2) && abs(temp1) >= abs(temp3)) {
            if (temp1 > 0)
                ++j;
            else
                ++i;
        } else if (abs(temp2) >= abs(temp1) && abs(temp2) >= abs(temp3)) {
            if (temp2 > 0)
                ++k;
            else
                ++j;
        } else if (abs(temp3) >= abs(temp1) && abs(temp3) >= abs(temp2)) {
            if (temp3 > 0)
                ++i;
            else
                ++k;
        }
    }
    return result;
}
/////////////////////
int Solution::minimize(const vector<int> &A, const vector<int> &B, const vector<int> &C) {
    int res = INT_MAX;
    for (int i = 0, j = 0, k = 0; i < A.size() && j < B.size() && k < C.size();) {
        int a = A[i], b = B[j], c = C[k];
        res = min(res, max( max(abs(a - b), abs(b - c)), abs(c - a)));
        if (a <= b && a <= c) i++;
        else if (b <= c && b <= a) j++;
        else k++;
    }
    return res;
}
```