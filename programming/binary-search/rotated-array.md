# Rotated Array

https://www.interviewbit.com/problems/rotated-array

 editorial */

int Solution::findMin(const vector<int> &A) {
    int low = 0, high = (int)A.size() - 1;
    int len = A.size();
    while (low <= high) {
        if (A[low] <= A[high]) return A[low]; // Case 1
        int mid = (low + high) / 2;
        int next = (mid + 1) % len, prev = (mid + len - 1) % len;
        if (A[mid] <= A[next] && A[mid] <= A[prev]) // Case 2
            return A[mid];
        else if (A[mid] <= A[high]) high = mid - 1; // Case 3
        else if (A[mid] >= A[low]) low = mid + 1; // Case 4
    }
    return -1;
}

/* minimal */

int Solution::findMin(const vector<int> &A) {
    int i = 0, j = A.size() - 1;
    while (i < j) {
        int mid = i + (j - i)/2;
        // If the minimum is on right side, continue looking right
        if (A[mid] > A[j]) i = mid + 1;
        else j = mid;
    }
    if (i == j) return A[i];
    return 0;
}


/* something(also works) */

int findMin1(const vector<int> & arr, int low, int high)  
{  
    // This condition is needed to  
    // handle the case when array is not  
    // rotated at all  
    if (high < low) return arr[0];  
  
    // If there is only one element left  
    if (high == low) return arr[low];  
  
    // Find mid  
    int mid = low + (high - low)/2; /*(low + high)/2;*/
  
    // Check if element (mid+1) is minimum element. Consider  
    // the cases like {3, 4, 5, 1, 2}  
    if (mid < high && arr[mid + 1] < arr[mid])  
    return arr[mid + 1];  
  
    // Check if mid itself is minimum element  
    if (mid > low && arr[mid] < arr[mid - 1])  
    return arr[mid];  
  
    // Decide whether we need to go to left half or right half  
    if (arr[high] > arr[mid])  
    return findMin1(arr, low, mid - 1);  
    return findMin1(arr, mid + 1, high);  
} 


int Solution::findMin(const vector<int> &A) {
    return findMin1(A, 0, A.size()-1);
}


/*
int Solution::findMin(const vector<int> &A) {
    int n=A.size(), low = 0, high = n - 1;
    while (low<=high) {
        if (A[low]<=A[high]) return low; // case 1
        int mid = (low+high)/2;
        int next = (mid+1) % n;
        int prev = (mid+n-1) % n;
        if (A[mid] <= A[next] && A[mid] <= A[prev]) return mid; // case 2
        else if (A[mid]<=A[high]) high = mid - 1;
        else if (A[mid]>=A[low]) low = mid + 1;
    }
    return -1;
}
## Solution

```cpp


```