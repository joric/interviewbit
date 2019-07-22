# Rotated Sorted Array Search

https://www.interviewbit.com/problems/rotated-sorted-array-search/

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7  might become 4 5 6 7 0 1 2 ).

You are given a target value to search. If found in the array, return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Input : [4 5 6 7 0 1 2] and target = 4

Output : 0

NOTE : Think about the case when there are duplicates. Does your current solution work? How does the time complexity change?*

## Hint 1

Think a modified version of the binary search. 
If the pivot is known, the binary search becomes trivial as the array to the either side of the pivot is sorted. 
Can you somehow search for the pivot in your binary search?

## Solution Approach

A naive solution is the linear search.

To improve, let us break our approach into 2 steps. First, we find the pivot (the index of minimum in the array).

Once we know the pivot, to search for x, we can do a conventional binary search in the left and right part of the pivot in the array.

Now, let us look at how binary search can be applied in this scenario to find the minimum.

There are 2 cases:

1)

```
          mid
   
           |
    
   6 7 8 9 1 2 3 4 5  
arr[mid] > arr[end]
```
The min lies in (mid, end]

Mid is excluded from the range because arr[mid] > arr[end]

2)
```
        
         mid
         
          | 
          
  6 7 8 9 1 2 3 4 5
arr[mid] < arr[end]
```
The min lies in [start, mid]

3) Note: If there are duplicates, making either decision might be difficult and hence linear search might be required.
```
               mid
               
                |
                
1 1 1 1 2 0 1 1 1 1 1 1 1 1 1 1 1 
arr[mid] = arr[end]
```
Indecisive. We would need to explore the whole array.


## Solution
```cpp
int Solution::search(const vector<int> &A, int B) {
    int n = A.size();
    int low = 0, high = n-1;
    while(low <= high){
        int mid = low + (high-low)/2;
        if(A[mid] == B) return mid;
        else if (A[0]<=A[mid]) { //i.e. left part is sorted
            if(A[0]<=B && B < A[mid]) high = mid-1;//B lies on left part
            else low = mid+1;
        } else { //right part is sorted
            if(A[mid] < B && B<=A[n-1]) low = mid+1;//B lies on right part
            else high = mid-1;
        }
    }
    return -1;
}
```

## Asked in
* Facebook
* Google
* Microsoft
* Amazon

