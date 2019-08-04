# First negative integer in window size B

https://www.interviewbit.com/problems/first-negative-integer-in-window-size-b


Given an array of integers A of size N and an integer B.
Find the first negative integer for each and every window(contiguous subarray) of size B.
If a window does not contain a negative integer, then return 0 for that window.

### Input Format

The arguments given are integer array A and integer B.

### Output Format

Return an integer array of size N+1-B representing answer of the ith window.

### Constraints

```
1 <= length of the array <= 200000
-10^9 <= A[i] <= 10^9 
```

### For Example

```
Input 1:
    A = [-1, 2, 3, -4, 5]
    B = 2
Output 1:
    [-1, 0, -4, -4] 

Input 2:
    A = [-8, 2, 3, -6, 10]
    B = 2
Output 2:
    [-8, 0, -6, -6]
```

## Hint 1

This problem is the variation of sliding window problem.

Try to think according to that perspective.

## Solution

### Editorial
```cpp
vector<int> Solution::solve(vector<int> &A, int B) {
    queue <int> neg;
    int n = A.size();
    for(int i=0;i<n;i++){
        if(A[i]<0){
            neg.push(i);
        }
    }
    int i=0;
    int j=B-1;
    vector<int> ans;
    for(;j<n;j++,i++){
        while(!neg.empty() && neg.front()<i){
            neg.pop();
        }
        if(neg.empty() || neg.front()>j){
            ans.push_back(0);
        }
        else{
            ans.push_back(A[neg.front()]);
        }
    }
    return ans;
}
```

### Fastest
```cpp
vector<int> doit(vector<int> &a, int k) {
    int n = a.size();
    vector<int> ans;
    deque<int> q;
    for (int i = 0; i < k; ++i)
        if (a[i] < 0) q.push_back(i);
    for (int i = k; i < n; ++i) {
        if (q.empty())
            ans.push_back(0);
        else
            ans.push_back(a[q.front()]);
        while (!q.empty() && q.front() < i - k + 1) q.pop_front();
        if (a[i] < 0) q.push_back(i);
    }
    if (!q.empty())
        ans.push_back(a[q.front()]);
    else
        ans.push_back(0);
    return ans;
}

vector<int> printFirstNegativeInteger(vector<int> &arr, int k) {
    vector<int> ans;
    for (int i = 0; i + k <= arr.size(); ++i) {
        int y = 0;
        for (int j = 0; j < k; ++j)
            if (arr[i + j] < 0) {
                y = arr[i + j];
                break;
            }
        ans.push_back(y);
    }
    return ans;
}

vector<int> Solution::solve(vector<int> &A, int B) {
    return printFirstNegativeInteger(A, B);
    //return doit(A,B);
}
```

### Lightweight

```cpp
vector<int> doit(vector<int> &a, int k) {
    int n = a.size();
    vector<int> ans;
    deque<int> q;
    for (int i = 0; i < k; ++i)
        if (a[i] < 0) q.push_back(i);
    for (int i = k; i < n; ++i) {
        if (q.empty())
            ans.push_back(0);
        else
            ans.push_back(a[q.front()]);
        while (!q.empty() && q.front() < i - k + 1) q.pop_front();
        if (a[i] < 0) q.push_back(i);
    }
    if (!q.empty())
        ans.push_back(a[q.front()]);
    else
        ans.push_back(0);
    return ans;
}

vector<int> Solution::solve(vector<int> &A, int B) {
    return doit(A, B);
}

```


### Mine

```cpp
vector<int> Solution::solve(vector<int> &arr, int k) {
    vector<int> res;
    deque<int>  q; 

    for (int i = 0; i < k; i++)
        if (arr[i] < 0) q.push_back(i);

    for (int i = k; i < arr.size(); i++)  { 
        res.push_back( q.empty() ? 0 : arr[q.front()] );
        while (!q.empty() && q.front() < i - k + 1) q.pop_front();
        if (arr[i] < 0) q.push_back(i);
    }

    res.push_back( q.empty() ? 0 : arr[q.front()] );
    return res;
}
```
