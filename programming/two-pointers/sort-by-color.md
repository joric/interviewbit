# Sort By Color

https://www.interviewbit.com/problems/sort-by-color



Given an array with n objects colored red, white or blue, 
sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note: Using library sort function is not allowed.

Example :

Input: [0 1 2 0 1 2]
Modify array so that it becomes: [0 0 1 1 2 2]

-

## Hint 1
Numbers in the array can only be 0,1 or 2. How can this be helpful?

Solution approach

There are multiple approaches possible here. We need to make sure we do not allocate extra memory.

Approach 1:

 Count the number of red, white and blue balls. 
Then in another pass, set initial redCount number of cells as 0, next whiteCount cell as 1
and next bluecount cells as 2. 
*Requires 2 pass of the array. * 

**Approach 2: **

 Swap the 0s to the start of the array maintaining a pointer, and 2s to the end of the array. 
1s will automatically be in their right position. 


## Solution

```cpp

// editorial

void Solution::sortColors(int A[], int n) {
    int n = A.size();
    int k = n - 1;
    int i = 0;
    for (; i < n; ++i) {
        if (A[i] != 0) {
            break;
        }
    }

    int j = i;
    for (; i <= k; ++i) {
        if (A[i] == 0) {
            swap(A[j++], A[i]);
        } else if (A[i] == 2) {
            swap(A[i--], A[k--]);
        }
    }
}

// lightweight

void Solution::sortColors(vector<int> &A) {
    int n = A.size();
    for (int i = 0; i < A.size(); ++i) {
        if (A[i] == 0)
            cout << A[i] << " ";
    }
    for (int i = 0; i < A.size(); ++i) {
        if (A[i] == 1)
            cout << A[i] << " ";
    }
    for (int i = 0; i < A.size(); ++i) {
        if (A[i] == 2)
            cout << A[i] << " ";
    }
    A.clear();
}

////////////////////////////

int twoPass(vector<int> &A, int n, int num) {
    int i = 0, j = n - 1;
    while (i < j) {
        if (A[j] == num)
            --j;
        else if (A[i] == num) {
            swap(A[i], A[j]);
            --j;
            ++i;
        } else
            ++i;
    }
    for (auto p = 0; p < n; ++p)
        if (A[p] == num)
            return p;
    return n;
}
void Solution::sortColors(vector<int> &A) {
    int n = A.size();
    int newSize = twoPass(A, n, 2);
    twoPass(A, newSize, 1);
}
```