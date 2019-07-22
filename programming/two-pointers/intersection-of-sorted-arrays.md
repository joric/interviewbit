# Intersection Of Sorted Arrays

https://www.interviewbit.com/problems/intersection-of-sorted-arrays


	Intersection Of Sorted Arrays
	Find the intersection of two sorted arrays.
	OR in other words,
	Given 2 sorted arrays, find all the elements which occur in both the arrays.
## Solution

```cpp
vector<int> Solution::intersect(const vector<int> &A, const vector<int> &B) {
    vector<int> res;
    int i=0,j=0,n=A.size(),m=B.size();
    while (i<n && j<m) {
        if (A[i]>B[j]) {
            j++;
        } else if (A[i]<B[j]) {
            i++;
        } else {
            res.push_back(A[i]);
            i++;
            j++;
        }
    }
    return res;
}
```