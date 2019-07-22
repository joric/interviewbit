# INVERSIONS

https://www.interviewbit.com/problems/inversions/

Given an array A, count the number of inversions in the array.

Formally speaking, two elements A[i] and A[j] form an inversion if A[i] > A[j] and i < j

Example:

```
A : [2, 4, 1, 3, 5]
Output : 3
```

as the 3 inversions are (2, 1), (4, 1), (4, 3).


## Solution


```cpp
// editorial

long long rec(vector<int> &v, int l, int r) {
    if (l >= r)
        return 0;
    int m = (l + r) / 2;
    long long count = rec(v, l, m);
    count += rec(v, m + 1, r);

    vector<int> aux(v.begin() + l, v.begin() + m + 1);
    int i = m + 1, j = 0;
    for (int k = l; k <= r; k++) {
        if (j == aux.size() || (i <= r && v[i] < aux[j])) {
            count += (int(aux.size()) - j);
            v[k] = v[i++];
        } else
            v[k] = aux[j++];
    }
    return count;
}

int Solution::countInversions(vector<int> &A) {
    return rec(A, 0, int(A.size()) - 1);
}

//////////////////////////////////

// lightweight

int merge(vector<int> &A, vector<int> &temp, int left, int mid, int right) {
    int i = left;
    int j = mid;
    int k = left;
    int inv_count = 0;
    while (i <= mid - 1 && j <= right) {
        if (A[i] <= A[j]) {
            temp[k++] = A[i++];
        } else {
            temp[k++] = A[j++];
            inv_count += mid - i;
        }
    }
    while (i <= mid - 1) {
        temp[k++] = A[i++];
    }
    while (j <= right) {
        temp[k++] = A[j++];
    }
    for (i = left; i <= right; i++)
        A[i] = temp[i];

    return inv_count;
}
int mergeSort(vector<int> &A, vector<int> &temp, int left, int right) {
    int inv_count = 0;
    int mid;
    if (left < right) {
        int mid = (left + right) / 2;
        inv_count = mergeSort(A, temp, left, mid);
        inv_count += mergeSort(A, temp, mid + 1, right);
        inv_count += merge(A, temp, left, mid + 1, right);
    }
    return inv_count;
}
int Solution::countInversions(vector<int> &A) {
    vector<int> temp(A.size());
    return mergeSort(A, temp, 0, A.size() - 1);
}

////////////////////////////////////////

int Merge(vector<int> &A, int start, int start2, int end) {
    vector<int> temp(end - start + 1);
    int k = 0;
    int i = start;
    int j = start2;
    int count = 0;

    while (i <= start2 - 1 && j <= end) {
        if (A[i] <= A[j]) {
            temp[k] = A[i];
            i++;
        }

        else {
            temp[k] = A[j];
            j++;

            count += start2 - i;
        }

        k++;
    }

    while (i <= start2 - 1) {
        temp[k] = A[i];
        k++;
        i++;
    }

    while (j <= end) {
        temp[k] = A[j];
        k++;
        j++;
    }

    for (int i = start; i <= end; i++) {
        A[i] = temp[i - start];
    }

    return count;
}

int countMerge(vector<int> &A, int start, int end) {
    if (start >= end)
        return 0;

    int mid = (start + end) / 2;

    return countMerge(A, start, mid) + countMerge(A, mid + 1, end) + Merge(A, start, mid + 1, end);
}

int Solution::countInversions(vector<int> &A) {
    return countMerge(A, 0, A.size() - 1);
}

```