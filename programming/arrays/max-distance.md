# Max Distance

https://www.interviewbit.com/problems/max-distance

Given an array A of integers, find the maximum of j - i subjected to the constraint of A[i] <= A[j].

If there is no solution possible, return -1.

Example :

A : [3 5 4 2]

Output : 2 
for the pair (3, 4)


## Solution

```cpp
/* O(nlogn) solution */

int Solution::maximumGap (const vector<int> &A) {
    int n = A.size();
    if (n == 0)
        return -1;
    if (n == 1)
        return 0;
    vector< pair<int, int> >pairs;
    for (int i = 0; i<n; i++)
        pairs.push_back (make_pair(A[i], i));
    sort (pairs.begin(), pairs.end());
    int maxIndex = pairs[n - 1].second;
    int ans = 0;
    for (int i = n - 2; i >= 0; i--) {
        ans = max (ans, maxIndex - pairs[i].second);
        maxIndex = max (maxIndex, pairs[i].second);
    }
    return ans;
}

/* O(n) solution */

int Solution::maximumGap(const vector<int> &A) {

    int n = A.size();

    if (n<=1)
        return 0;

    vector<int> arr = A;

    int maxDiff = -1;
    int i, j;

    int *LMin = (int *)malloc(sizeof(int)*n); 
    int *RMax = (int *)malloc(sizeof(int)*n); 

   /* Construct LMin[] such that LMin[i] stores the minimum value 
       from (arr[0], arr[1], ... arr[i]) */
    LMin[0] = arr[0]; 
    for (i = 1; i < n; ++i) 
        LMin[i] = min(arr[i], LMin[i-1]); 
  
    /* Construct RMax[] such that RMax[j] stores the maximum value 
       from (arr[j], arr[j+1], ..arr[n-1]) */
    RMax[n-1] = arr[n-1]; 
    for (j = n-2; j >= 0; --j) 
        RMax[j] = max(arr[j], RMax[j+1]); 
  
    /* Traverse both arrays from left to right to find optimum j - i 
        This process is similar to merge() of MergeSort */
    i = 0, j = 0, maxDiff = -1; 
    while (j < n && i < n) 
    { 
        if (LMin[i] <= RMax[j]) 
        { 
            maxDiff = max(maxDiff, j-i); 
            j = j + 1; 
        } 
        else
            i = i+1;
    } 
    return maxDiff;
}
```
