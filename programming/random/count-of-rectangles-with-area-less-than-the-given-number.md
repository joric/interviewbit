# Count of rectangles with area less than the given number

https://www.interviewbit.com/problems/count-of-rectangles-with-area-less-than-the-given-number/


Given a sorted array of distinct integers A and an integer B, 
find and return how many rectangles with distinct configurations can be 
created using elements of this array as length and breadth whose area 
is lesser than B.

(Note that a rectangle of 2 x 3 is different from 3 x 2 if we take configuration into view)

```
For example:

A = [2 3 5],  B = 15
Answer = 6 (2 x 2, 2 x 3, 2 x 5, 3 x 2, 3 x 3, 5 x 2)
Note: As the answer may be large return the answer modulo (10^9 + 7).


Input Format

The first argument given is the integer array A.
The second argument given is integer B.
Output Format

Return the number of rectangles with distinct configurations with area less than B modulo (10^9 + 7).
Constraints

1 <= length of the array <= 100000
1 <= A[i] <= 10^9 
1 <= B <= 10^9
For Example

Input 1:
    A = [1, 2, 3, 4, 5]
    B = 5
Output 1:
    8

Input 2:
    A = [5, 10, 20, 100, 105]
    B = 110
Output 2:
    6
```

## Solution

### Editorial
```cpp
int Solution::solve(vector<int> &a, int b) {
    long ans = 0, mod = (long)(1e9 + 7);
    int l = 0, r = a.size() - 1;
    while(l < a.size() && r >= 0) {
        if(1L * a[l] * a[r] < b) {
            ans = (ans + r + 1) % mod;
            l++;
        } else  r--;
    }
    return (int)ans;
}

```

### Fastest
```cpp
typedef long long int ll;

int Solution::solve(vector<int> &A, int B) {
    int s = A.size();
    unsigned long int mod = 1000000007;
    int ans = 0;
    int i,j;
    i=0;
    j=s-1;
    ll z = B;
    while(i<=j)
    {
        long long int temp = (ll)((long long)A[i]*(long long)A[j]);
        if(temp>=B)
        {
            j--;
        }
        else
        {
            int l = (j-i+1)%mod;
            ans =(ans%mod+(((2*l)%mod)-1)%mod)%mod;
            i++;
        }
    }
    if(ans<0)
    {
        ans+=mod;
    }
    return (ll)(ans%mod);
}
```

### Lightweight
```cpp
int Solution::solve(vector<int> &a, int area) {
    long long int mul;
    area = area % (1000000000 + 7);
    long long n = a.size();
    long long  l = 0,r = n-1 , ans = 0;
    while(l <= r){
        mul = (long long int)(a[l]*a[r]);
        if(mul < area){
            ans = ans + 2*(r-l) + 1;
            ans%=1000000007;
             l++;
        }
        else
            r--;
    }
    return ans ;
}
```

### Mine
```cpp
#define mod 1000000007
int Solution::solve(vector<int> &A, int B) {
    int n = A.size();
    int l=0, r = n-1;
    int count = 0;
    long long k = (long long)B;

    while(l < r) {
        long long a = (long long)A[l];
        long long b = (long long)A[r];
        long long res = a * b;
        if (res < k) {
            if (a!=b) {
                count += (r - l) * 2;
                count = count % mod;
            }
            l++;
        } else {
            r--;
        }
    }

    for (long long x:A) {
        if (x*x<k) {
            count++;
            count = count % mod;
        }
    }

    return count;
}

```
