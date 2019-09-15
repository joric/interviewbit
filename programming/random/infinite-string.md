# INFINITE STRING

https://www.interviewbit.com/problems/infinite-string/

Given a string of lowercase alphabets A of size N.

Find and return the number of substrings in A having more than 1 distinct alphabets % 10^7.

### Input Format

The first argument given is string A.

### Output Format

Return the number of substrings in A having more than 1 distinct alphabets % 10^7.

### Constraints

1 <= N <= 100000

### For Example

```
Input 1:
    A = "abcd"
Output 1:
    6

Input 2:
    A = "abcde"
Output 2:
    10
```
## Solution
### Editorial
```cpp
#define mod (ll)10000000
#define ll long long

int Solution::solve(string  A)
{

    int n  = A.length();
    ll ptr1 = 0,ptr2 = 0;
    long long ans =0 ;
    while(ptr2<n)
    {
     if(A[ptr1]==A[ptr2])
     {
         ptr2++;

     }
     else
     {
         ans+=n-ptr2;
         ptr1++;
     }
    }
    return ans%mod;
}
```

### Fastest
```cpp
int bc(int n, int k)
{
    return n * (n-1) / 2;
}
int Solution::solve(string A) {
    const int n =  A.length();
    int d = 0, add = 1;
    for (int i = 1; i < n; ++i) {
        if (A[i] == A[i-1])
            d += add, add++;
        else
            add = 1;
    }
    return ((long)n * (n-1) / 2 - d) % 10000000;
}
```

