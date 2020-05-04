# Search for a Range

https://www.interviewbit.com/problems/search-for-a-range




Given a sorted array of integers, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

Example:

Given [5, 7, 7, 8, 8, 10]

and target value 8,

return [3, 4].

## Hint 1:

The problem can be simply broken down as two binary searches for the begining and end of the range, respectively.

First, let us find the left boundary of the range.

We initialize the range to [i=0, j=n-1]. In each step, calculate the middle element [mid = (i+j)/2].

Now according to the relative value of A[mid] to target, there are three possibilities:

If A[mid] < target, then the range must begin on the right of mid (hence i = mid+1 for the next iteration)

If A[mid] > target, it means the range must begin on the left of mid (j = mid-1)

If A[mid] = target, then the range must begins on the left of or at mid (j= mid)

Since we would move the search range to the same side for case 2) and 3), we might as well merge them as one single case so that less code is needed:

2*. If A[mid] >= target, j = mid;

Surprisingly, 1 and 2* are the only logic you need to put in loop while (i < j). When the while loop terminates, the value of i/j is where the start of the range is. Why?

No matter what the sequence originally is, as we narrow down the search range, eventually we will be at a situation where there are only two elements in the search range.

Suppose our target is 5, then we have only 7 possible cases:

Case 1: [5 7] (A[i] = target < A[j])

Case 2: [5 3] (A[i] = target > A[j])

Case 3: [5 5] (A[i] = target = A[j])

## Solution

```cpp

int binarySearch(const vector<int>& A, int key, bool searchFirst){
    int start = 0;
    int end = A.size()-1;
    int mid;
    int result =  -1;
    while(start <= end){
        mid = start + (end-start)/2;
        if(A[mid] == key){
            result = mid;
            if(searchFirst){
                end = mid-1;
            }
            else{
                start = mid+1;
            }
        }
        else if(A[mid] > key){
            end = mid-1;
        }
        else{ // A[mid] < key
            start = mid+1;
        }
    }
    return result;
}

 
vector<int> Solution::searchRange(const vector<int> &A, int B) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
    vector<int> sol;
    
    sol.push_back(binarySearch(A, B, true));
    sol.push_back(binarySearch(A, B, false));
    
    return sol;
    
}
```

## Another solution

```cpp
vector<int> Solution::searchRange(const vector<int> &A, int B) {
    auto r = equal_range(A.begin(), A.end(), B);
    if (r.first!=A.end() && *r.first!=B)
        return {-1, -1};
    return { r.first-A.begin(), r.second-A.begin()-1 };
}
```

