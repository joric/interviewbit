# Minimum Swaps 2

https://www.interviewbit.com/problems/minimum-swaps-2/


Given an array of integers A of size N that is a permutation of [0, 1, 2, …, (N-1)]. 
It is allowed to swap any two elements (not necessarily consequtive).

Find the minimum number of swaps required to sort the array in ascending order.


```
Input Format

The only argument given is the integer array A.
Output Format

Return the minimum number of swaps.
Constraints

1 <= N <= 100000
0 <= A[i] < N
For Example

Input 1:
    A = [1, 2, 3, 4, 0]
Output 1:
    4

Input 2:
    A = [2, 0, 1, 3]
Output 2:
    2
```

## Solution

### Editorial
```cpp
int Solution::solve(vector<int> &a) {
    int n = a.size();
    int b[n];
    for(int i = 0; i < n; i++)  b[a[i]] = i;
    int ans = 0;
    for(int i = 0; i < n; i++) {
        if(a[i] != i) {
            int t = a[i];
            a[i] = i;
            a[b[i]] = t;
            b[t] = b[i];
            b[i] = i;
            ans++;
        }
    }
    return ans;
}

```

### Fastest
```cpp
int Solution::solve(vector<int> &A) {
    int swap=0;
    for(int i=0;i<A.size();i++) {
        while(A[i]!=i) {
            int tmp=A[i];
            int tmp2 = A[A[i]];
            A[i]=tmp2;
            A[tmp]=tmp;
            swap++;
        }
    }
    return swap;
    
}

```

### Lightweight
```cpp
int Solution::solve(vector<int> &A) {
    int i=0, count=0;;
    while(i<A.size()) {
        if(A[i]==i)
            i++;
        else {
            swap(A[i], A[A[i]]);
            count++;
        }
    }
    return count;
}
```

### Arbitrary array
```cpp
int Solution::solve(vector<int> &arr) {
    int n = arr.size();
    vector<pair<int, int>> vec(n);

    for (int i = 0; i < n; i++) {
        vec[i].first = arr[i];
        vec[i].second = i;
    }

    sort(vec.begin(), vec.end());

    int res = 0, c = 0, j;

    for (int i = 0; i < n; i++) {
        if (vec[i].second == i)
            continue;
        else {
            swap(vec[i].first, vec[vec[i].second].first);
            swap(vec[i].second, vec[vec[i].second].second);
        }
        if (i != vec[i].second)
            --i;
        res++;
    }

    return res;
}

```

