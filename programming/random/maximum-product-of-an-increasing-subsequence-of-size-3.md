# Maximum product of an increasing subsequence of size 3

https://www.interviewbit.com/problems/maximum-product-of-an-increasing-subsequence-of-size-3/

Given an array of integers A of size N. 
Your task is to find maximum product of increasing subsequence of size 3 
i.e. you need to find maximum value of A[i] * A[j] * A[k] such that A[i] < A[j] < A[k] and i < j < k < N for all 
increasing subsequences of size 3.

If there is no increasing subsequence of size 3 return -1, 
else return maximum product of increasing subsequence of size 3 modulo 109+7.

Note: All elements of the array A are distinct.

### Input Format

The only argument given is the integer array A.

### Output Format

If there is no increasing subsequence of size 3 return -1, 
else return the maximum product of increasing subsequence of size 3 modulo 10^9+7.

### Constraints

```
1 <= N <= 100000
1 <= A[i] <= 10^6 
```
### For Example

```
Input 1:
    A = [10, 11, 9, 5, 6, 1, 20]
Output 1:
    2200
    Explanation 1:
        Maximum product is achieved when i=0, j=1, k=6 i.e A[0] * A[1] * A[6] = 10 * 11 * 20 = 2200.
        

Input 2:
    A = [5, 4, 3, 2, 1]
Output 2:
    -1
    Explanation 2:
        There is no increasing subsequence of size 3.
```

## Solution

### Editorial
```cpp
const int mod = 1000000007;

int bruteforce(vector<int> &a){
    int n=a.size();
    long long ans=-1;
    for(int i=0; i+2<n; ++i)
        for(int j=i+1; j+1<n; ++j)
            for(int k=j+1; k<n; ++k){
                    if(a[i]<a[j]&&a[j]<a[k]){
                        long long temp=1LL*a[i]*a[j]*a[k];
                        if(ans<temp)
                            ans=temp;
                    }
                }
    if(ans!=-1)
        ans%=mod;
    return (int)(ans);
}

int solveit(vector<int> &a){
    //return bruteforce(a);
    int n=a.size();
    long long ans=-1;
    long long smaller[n];
    set<long long> s;
    memset(smaller,-1,sizeof smaller);
    set<long long>::iterator it;

    for(int i=0; i<n; ++i){
        s.insert(a[i]);
        it=s.lower_bound(a[i]);
        if(it!=s.begin()){
            --it;
            smaller[i]=*it;
        }
    }
    long long suffixmax=a[n-1];
    for(int i=n-2; i>0; --i){
        if(a[i]>suffixmax)
            suffixmax=a[i];
        else if(smaller[i]!=-1){
            long long temp=1LL*smaller[i]*a[i]*suffixmax;
            if(ans<temp)
                ans=temp;
        }
    }
    if(ans!=-1)
        ans%=mod;
    return (int)(ans);
}


int Solution::solve(vector<int> &A) {
    return solveit(A);
}
```

### Fastest
```cpp
const int mod = 1000000007;


int solveit(vector<int> &a){
    int n=a.size();
    long long ans=-1;
    long long smaller[n];
    set<long long> s;
    memset(smaller,-1,sizeof smaller);
    set<long long>::iterator it;

    for(int i=0; i<n; ++i){
        s.insert(a[i]);
        it=s.lower_bound(a[i]);
        if(it!=s.begin()){
            --it;
            smaller[i]=*it;
        }
    }
    long long suffixmax=a[n-1];
    for(int i=n-2; i>0; --i){
        if(a[i]>suffixmax)
            suffixmax=a[i];
        else if(smaller[i]!=-1){
            long long temp=1LL*smaller[i]*a[i]*suffixmax;
            if(ans<temp)
                ans=temp;
        }
    }
    if(ans!=-1)
        ans%=mod;
    return (int)(ans);
}


int Solution::solve(vector<int> &A) {
    return solveit(A);
}
```

### Lightweight
```cpp
int Solution::solve(vector<int> &A) {
    set<long long> l;
    int Max = A[A.size()-1];
    long long ans = -1;
    for( int i = 0; i <= A.size()-2; i++ )l.insert(A[i]);
    for( int i = A.size()-2; i > 0; i-- ){
        l.erase(A[i]);
        auto it = l.upper_bound(A[i]);
        if( it != l.begin() && Max > A[i] ){
            it--;
            long long num = *it;
            ans = max( ans, num*1LL*A[i]*1LL*Max );
        }
        Max = max( Max, A[i] );
    }
    if( ans != -1 )ans %= 1000000007;
    return ans;
}
```

### Mine
```cpp
int Solution::solve(vector<int> &a) {
    long long res = -1;
    int n = a.size();
    int right = a[n-1];    
    set<long long> s;
    for (int i = 0; i<n-3; i++)
        s.insert(a[i]);
    for (int i = n-2; i > 0; i--) {
        s.erase(a[i]);
        auto it = s.upper_bound(a[i]);
        if (it != s.begin() && right > a[i] ) {
            it--;
            long long left = *it;
            res = max(res, left * a[i] * right );
        }
        right = max(right, a[i]);
    }
    return res == -1 ? -1 : res % 1000000007;
}
```
