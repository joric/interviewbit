# Good Subarrays

https://www.interviewbit.com/problems/good-subarrays/

You are given an array A of size n. Each element of the array is a positive integer.

A subarray defined by `(i, j)` is called a good-subarray if the number of distint elements in
`(A[i], A[i+1], ...., A[j])` is not greater than B.

You are asked to tell the number of good-subarrays of each length 1 to n for the given array.

### Input Format

* The first argument is the array A
* The second argument denotes the value B

### Output Format

* Return an array of integers where i'th integer denotes the number of good-subarrays of length (i+1)

### Constraints

* 1 <= n, A[i] <= 100000
* 1 <= B <= n

### For Example
```
Input 1:
    A = [1, 2, 2, 3, 1]
    B = 2
    
Output 1:
    5 4 2 0 0

Explanation:
    For length 1: [1], [2], [2], [3], [1] => Count = 5
    For length 2: [1, 2], [2, 2], [2, 3], [3, 1] => Count = 4
    For length 3: [1, 2, 2], [2, 2, 3] => Count = 2
    For length 4: => Count = 0
    For length 5: => Count = 0
```
## Solution Approach

Two pointer technique can be used to find good subarray of maximum size starting from each index.

It can easily be observed that each subarray of a good subarray will also have at most B distinct elements.

Hence, the count for greater length good subarrays can be added to the count of smaller length good subarrays.

## Solution

### Editorial
```cpp
const int N = 1e5 + 10;
int f[N];

vector<int> Solution::solve(vector<int> &A, int B) {
    
    vector< int > res;
    int j = 0, cnt = 0, n = A.size();
    
    assert(1 <= n);
    assert(n <= 100000);
    assert(1 <= B);
    assert(B <= n);
    
    res.resize(n);
    memset(f, 0, sizeof(f));
    
    for(int i = 0; i < n; i++) {
        assert(A[i] >= 1);
        assert(A[i] <= 100000);
        while( j < n && cnt + !f[A[j]] <= B ) {
            cnt += !f[A[j]];
            f[A[j]]++;
            j++;
        }

        res[j-i-1]++;
        
        f[A[i]]--;
        if( f[A[i]] == 0 ) cnt--;
    }



    for( int i = n-2; i >= 0; i-- )
        res[i] += res[i+1];

    return res;
    
}
```

### Fastest
```cpp
// same as editorial
```

### Lightweight
```cpp
vector<int> Solution::solve(vector<int> &A, int B) {
    vector<int> f(100000,0);
    int cnt = 0;int j=0;
    vector<int> res(A.size(),0);
    for(int i=0;i<A.size();i++){
        while(j<A.size() && cnt+!f[A[j]]<=B){
            cnt += !f[A[j]];
            f[A[j]]++;
            j++;
        }
        res[j-i-1]++;
        f[A[i]]--;
        if(f[A[i]]==0) cnt--;
    }
    for(int i=A.size()-2;i>=0;i--){
        res[i] += res[i+1];
    }
    return res;
}
```

