# Max Non-Negative Subarray

https://www.interviewbit.com/problems/max-non-negative-subarray/

Find out the maximum sub-array of non negative numbers from an array.
The sub-array should be continuous. That is, a sub-array created by choosing the second and fourth element and skipping the third element is invalid.

Maximum sub-array is defined in terms of the sum of the elements in the sub-array. Sub-array A is greater than sub-array B if sum(A) > sum(B).

Example:

A : [1, 2, 5, -7, 2, 3]

The two sub-arrays are [1, 2, 5] [2, 3].

The answer is [1, 2, 5] as its sum is larger than [2, 3]

NOTE: If there is a tie, then compare with segment's length and return segment which has maximum length

NOTE 2: If there is still a tie, then return the segment with minimum starting index



## Solution

```cpp
vector<int> Solution::maxset(vector<int> &A) {
   int len = A.size();  
   long long maxSum = 0;  
   long long curSum = 0;  
   int startMax = -1;  
   int endMax = -1;  
   int start = 0;  
   int end = 0;  
   while(end < len) {  
    if(A[end] >= 0) {  
      curSum += (long long)A[end];  
      if(curSum > maxSum) {  
           maxSum = curSum;  
           startMax = start;  
           endMax = end + 1;  
      } else if(curSum == maxSum) {  
           if(end + 1 - start > endMax - startMax) {  
               startMax = start;  
              endMax = end + 1;  
           }  
      }  
    }else {  
      start = end + 1;  
      curSum = 0;  
    }  
    end++;  
   }  
   vector<int> ans;  
   ans.clear();  
   if(startMax == -1 || endMax == -1)  
     return ans;  
   for(int i = startMax; i < endMax; ++i)  
      ans.push_back(A[i]);  
   return ans; 
}
```

## Asked in

* Google
