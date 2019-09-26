# Reverse pairs

https://www.interviewbit.com/problems/reverse-pairs/


Given an array of integers A, we call (i, j) an important reverse pair if i < j and A[i] > 2*A[j].

Return the number of important reverse pairs in the given array A.

### Input Format

The only argument given is the integer array A.
### Output Format

Return the number of important reverse pairs in the given array A.
### Constraints
```
1 <= length of the array <= 100000
1 <= A[i] <= 10^9 
```
### For Example
```
Input 1:
    A = [1, 3, 2, 3, 1]
Output 1:
    2

Input 2:
    A = [2, 4, 3, 5, 1]
Output 2:
    3
```

## Solution
### Editorial
```cpp
int merge(vector<int>& left, vector<int>& right, vector<int>& A){
    int l=0,r=0,lsize=left.size(),rsize=right.size();
    int smaller=0,count=0;
    /// for each left, I have to see the right part from begin
    while(l<lsize){
        while(r<rsize && left[l]>2ll*right[r])
            r++;
        count += (r);
        l++;
    }
    l=0,r=0;
    while(l<lsize && r<rsize){
        if(left[l]<=right[r]){
            A.push_back(left[l++]);
        }
        else{
            A.push_back(right[r++]);
        }
    }
    
    while(l<lsize){
        A.push_back(left[l++]);
    }
    
    while(r<rsize){
        A.push_back(right[r++]);
    }
    return count;
}

int merge_sort(vector<int>& A){
    int size = A.size();
    if(size<=1) return 0;
    vector<int> left, right;
    left.assign(A.begin(), A.begin()+size/2);
    right.assign(A.begin()+size/2, A.end());
    int count = 0; /// reverse pair
    A.clear();
    count += merge_sort(left);
    count += merge_sort(right);
    count += merge(left, right, A);
    return count;
}

int Solution::solve(vector<int> &A){
    /// this is one the variation of merge sort;
    /// naive solution is O(n^2)
    /// using merge sort O(nlogn)
    return merge_sort(A);
}
```

### Fastest
```cpp
void merge(vector<int>& A, int start, int mid, int end)
{
    int n1 = (mid - start + 1);
    int n2 = (end - mid);
    int L[n1], R[n2];
    for (int i = 0; i < n1; i++)
        L[i] = A[start + i];
    for (int j = 0; j < n2; j++)
        R[j] = A[mid + 1 + j];
    int i = 0, j = 0;
    for (int k = start; k <= end; k++) {
        if (j >= n2 || (i < n1 && L[i] <= R[j]))
            A[k] = L[i++];
        else
            A[k] = R[j++];
    }
}

int mergesort_and_count(vector<int>& A, int start, int end)
{
    if (start < end) {
        int mid = (start + end) / 2;
        int count = mergesort_and_count(A, start, mid) + mergesort_and_count(A, mid + 1, end);
        int j = mid + 1;
        for (int i = start; i <= mid; i++) {
            while (j <= end && A[i] > A[j] * 2LL)
                j++;
            count += j - (mid + 1);
        }
        merge(A, start, mid, end);
        return count;
    }
    else
        return 0;
}
int Solution::solve(vector<int> &nums) {
    return mergesort_and_count(nums,0,nums.size()-1);
}
```

### Lightweight
```cpp
//LeetCdoe Solution: (Discussion)
int sort_and_count(vector<int>::iterator begin, vector<int>::iterator end) {
    if (end - begin <= 1)
        return 0;
    auto mid = begin + (end - begin) / 2;
    int count = sort_and_count(begin, mid) + sort_and_count(mid, end);
    for (auto i = begin, j = mid; i != mid; ++i) {
        while (j != end and *i > 2L * *j)
            ++j;
        count += j - mid;
    }
    inplace_merge(begin, mid, end);
    return count;
}
int Solution::solve(vector<int> &nums) {
    return sort_and_count(nums.begin(), nums.end());
}
```

## References
* https://leetcode.com/problems/reverse-pairs/

