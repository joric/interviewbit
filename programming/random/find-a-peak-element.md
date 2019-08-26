# Find a peak element

https://www.interviewbit.com/problems/find-a-peak-element/

Given an array of integers A, find and return the peak element in it.
An array element is peak if it is NOT smaller than its neighbors. 
For corner elements, we need to consider only one neighbor. 
For example, for input array {5, 10, 20, 15}, 20 is the only peak element.

Following corner cases give better idea about the problem.

1. If input array is sorted in strictly increasing order, the last element is always a peak element. 
For example, 5 is peak element in {1, 2, 3, 4, 5}.

2. If input array is sorted in strictly decreasing order, the first element is always a peak element. 

10 is the peak element in {10, 9, 8, 7, 6}.

### Note

It is guranteed that the answer is unique.

### Input Format

The only argument given is the integer array A.

### Output Format

Return the peak element.

### Constraints
```
1 <= length of the array <= 100000
1 <= A[i] <= 10^9 
```
### For Example
```
Input 1:
    A = [1, 2, 3, 4, 5]
Output 1:
    5

Input 2:
    A = [5, 17, 100, 11]
Output 2:
    100
```
## Solution 
### Editorial
```cpp
int Solution::solve(vector<int> &A) {
    int n = A.size();
    /*if(n == 1) return A[0];
    if(A[0]>=A[1]) return A[0];
    for(int i=1;i<n-1;i++){
        if(A[i] >= A[i-1] && A[i] >= A[i+1]) return A[i];
    }
    if(A[n-1]>=A[n-2]) return A[n-1];*/
    int l  = 0,r= n-1;
    while(l <=r){
        int mid = l + (r-l)/2;
        if ((mid == 0 || A[mid-1] <= A[mid]) && 
            (mid == n-1 || A[mid+1] <= A[mid])) 
        return A[mid];
        else if( mid > 0 && A[mid] <= A[mid-1]){
            r = mid-1;
        }
        else {
            l = mid+1;
        }
    }
}
```
### Fastest
```cpp
int Solution::solve(vector<int> &arr)
{
    int l=0, r=arr.size();
    while(l<=r)
    {
        int m=(l+r)/2;
        if(arr[m]>=arr[m+1] && arr[m]>=arr[m-1])
            return arr[m];
        if(arr[m]>arr[m-1])
            l=m;
        else
            r=m;
    }
}
```

### Lightweight
```cpp
int Solution::solve(vector<int> &A) 
{
    if(A[0]>A[1])
        return A[0];
    for(int i=1;i<A.size()-1;i++)
        if(A[i]>A[i-1]&&A[i]>A[i+1])
            return A[i];
    if(A[A.size()-1]>A[A.size()-2])
        return A[A.size()-1];
}
```
### Mine
```cpp
int Solution::solve(vector<int> &A) {
    sort(A.rbegin(), A.rend());
    return A[0];
}
```

