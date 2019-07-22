# Remove Duplicates From Sorted Array II

https://www.interviewbit.com/problems/remove-duplicates-from-sorted-array-ii


## Solution

```cpp

int Solution::removeDuplicates(vector<int> &A) {
    int count = 0;
    int size = A.size();
    for (auto i = 0; i < size; ++i) {
        if (i < size - 2 && A[i] == A[i + 1] && A[i] == A[i + 2])
            continue;
        else
            A[count++] = A[i];
    }
    return count;
}

int Solution::removeDuplicates(vector<int> &A) {
    int i = 0, j = A.size() - 1, k = 0;
    while (i <= j) {
        A[k] = A[i++];
        while (i + 1 <= j && A[i] == A[k] && A[i + 1] == A[k])
            i++;
        k++;
    }
    return k;
}
```