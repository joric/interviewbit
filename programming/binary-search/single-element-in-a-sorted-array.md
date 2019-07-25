# Single Element in a Sorted Array

https://www.interviewbit.com/problems/single-element-in-a-sorted-array/

Given a sorted array of integers A where every element appears twice except for one element which appears once, 
find and return this single element that appears only once.

```
Input Format

The only argument given is the integer array A.
Output Format

Return the single element that appears only once.
Constraints

1 <= length of the array <= 100000
1 <= A[i] <= 10^9 
For Example

Input 1:
    A = [1, 1, 2, 2, 3]
Output 1:
    3

Input 2:
    A = [5, 11, 11, 100, 100]
Output 2:
    5
```

## Solution
### Editorial
```cpp
int Solution::solve(vector<int> &A) {
    int n = A.size();
    int st = 0, en = n-1, m;
    while (st <= en)
    {
        m = (st+en)/2;
        if (m == 0)
        {
            if (A[m] != A[m+1])     return A[m];
            else                    st = m+2;
        }
        else if (m == n-1)
        {
            if (A[m] != A[m-1])     return A[m];
            else                    en = m-2;
        }
        else if (A[m-1] != A[m] && A[m] != A[m+1])  return A[m];
        else if (A[m-1] != A[m])    // it is first occ
        {
            if (m%2 == 0)   st = m+2;
            else            en = m-1;
        }
        else                        // second occ
        {
            if (m%2 == 0)   en = m-2;
            else            st = m+1;
        }
    }
}
```
### Fastest
```cpp
int Solution::solve(vector<int> &arr) {
    int si=0, ei = arr.size()-1;
    while(si<ei){
        int mid = (si+ei)/2;
        if(arr[mid] == arr[mid-1]){
            if((mid-1-si)%2 == 0) si = mid+1;
            else ei = mid-2;
        }
        else if(arr[mid] == arr[mid+1]){
            if((ei-mid-1)%2 == 1) si = mid+2;
            else ei = mid-1;
        }
        else{
            return arr[mid];
        }
    }
    return arr[si];
}

```
### Lightweight
```cpp
int Solution::solve(vector<int> &A) {
    set<long long int> st;
    for(int i=0;i<A.size();i++)
    {   auto it=st.find(A[i]);
        if(it==st.end())
            st.insert(A[i]);
        else
            st.erase(it);
    }
    return *st.begin();
}
```
### Mine
```cpp
int Solution::solve(vector<int> &str) {
    int c = 1, n = str.size();
    for (int i=0; i<n-1; i++) {
        if (str[i]==str[i+1]) {
            c++;
        } else {
            if (c==1)
                return str[i];
            c = 1;
        }
    }
    if (c==1 && n)
        return str[n-1];
    return -1;
}
```


