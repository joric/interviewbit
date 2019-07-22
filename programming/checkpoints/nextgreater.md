# NEXTGREATER

https://www.interviewbit.com/problems/nextgreater/

Given an array, find the next greater element G[i] for every element A[i] in the array. The Next greater Element for an element A[i] is the first greater element on the right side of A[i] in array. 
More formally,
```
G[i] for an element A[i] = an element A[j] such that 
    j is minimum possible AND 
    j > i AND
    A[j] > A[i]
```
Elements for which no greater element exist, consider next greater element as -1.

Example:

Input : A : [4, 5, 2, 10]

Output : [5, 10, 10, -1]

Example 2:

Input : A : [3, 2, 1]

Output : [-1, -1, -1]


## Solution

```cpp
vector<int> Solution::nextGreater(vector<int> &A) {
    int n = A.size();
    vector<int> nG(n);
    nG[n - 1] = -1;
    for (int i = n - 1; i > 0; i--) {
        if (A[i] > A[i - 1]) {
            nG[i - 1] = A[i];
        } else {
            if (A[i - 1] < nG[i]) {
                nG[i - 1] = nG[i];
            } else {
                int j = i;
                while (j < n && A[i - 1] >= nG[j++])
                    ;
                nG[i - 1] = nG[j - 1];
            }
        }
    }
    return nG;
}
```
