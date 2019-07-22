# Sorted Insert Position

https://www.interviewbit.com/problems/sorted-insert-position


Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Here are few examples.
```
[1,3,5,6], 5 -> 2
[1,3,5,6], 2 -> 1
[1,3,5,6], 7 -> 4
[1,3,5,6], 0 -> 0
```

## Hint 1

You need to return the index of least element >= x.

## Hint 2

Note that this is classic binary search. Instead of looking for the element x, 
you are looking for the least elements >= x.
## Solution

```cpp

/* editorial */

class Solution {
    public:
        int searchInsert(vector<int> &A, int target) {
            int n = A.size();
            int start = 0, end = n - 1;
            int mid;
            while(start <= end){
                mid = (start + end) / 2;
                if(target == A[mid]){
                    return mid;
                }
                else if(target < A[mid]){
                    end = mid - 1;
                }
                else{
                    start = mid + 1;
                }
            }
            return start;
        }
};

/* my solution */

int Solution::searchInsert(vector<int> &A, int B) {
    return lower_bound(A.begin(), A.end(), B) - A.begin();
}
```