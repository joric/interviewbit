# Merge Two Sorted Lists II

https://www.interviewbit.com/problems/merge-two-sorted-lists-ii


	Merge Two Sorted Lists II
	Given two sorted integer arrays A and B, merge B into A as one sorted array.
## Solution

```cpp

#include <bits/stdc++.h>
using namespace std;
struct Solution { static void merge(vector<int>&a, vector<int>&b); };
int main() {
	vector<int> a={1,2,3,4,5}, b = {1,4,5,8};
	Solution::merge(a, b);
	for (auto x:a) cout<<x<<" "; cout<<endl;
}

#if 1
/* editorial */
void Solution::merge(vector<int>&a, vector<int>&b) {
    int i = 0, j = 0, k=0, n=a.size(), m=b.size();
    vector <int> c(n+m);
    while (i<n && j<m) c[k++] = a[i] < b[j] ? a[i++] : b[j++];
    while (i<n) c[k++] = a[i++];
    while (j<m) c[k++] = b[j++];
    a = c;
}
#endif

#if 0
/* my solution */
void Solution::merge(vector<int> &A, vector<int> &B) {
    int n = A.size();
    int m = B.size();
    vector<int> res(n+m);
    for (int i=0,j=0,k=0; k<n+m; k++) {
        if (i<n && j<m) {
            res[k] = A[i]<B[j] ? A[i++] : B[j++];
        } else if (i<n) {
            res[k] = A[i++];
        } else if (j<m)
            res[k] = B[j++];
    }
    A = res;
}
#endif

#if 0
/* shorter */
void Solution::merge(vector < int >&a, vector < int >&b) {
    int i = 0, j = 0, k=0, n=a.size(), m=b.size();
    vector <int> c;
    for (;i<n && j<m; c.push_back(a[i] < b[j] ? a[i++] : b[j++]));
    for (;i<n;c.push_back(a[i++]));
    for (;j<m;c.push_back(b[j++]));
    a = c;
}
#endif

```