## Array and Prime Divisors

https://www.interviewbit.com/problems/array-and-prime-divisors/

You are given an array A having N positive distinct integers and an integer B.
You have to find the number of prime divisors of B present in A.

### Input Format

First argument given is an Array A, having N integers.
Second argument given is an integer B. 

### Output Format

Return a single integer, i.e number of prime divisors of B present in A

### Constraints

```
1 <= N <= 1e4
1 <= A[i] <= 1e5
1 <= B <= 1e6
```

### For Example

```
Input:
    A = [1, 2, 3, 4, 5]
    B = 6
Output:
    2
```

### Explanation
```
prime divisors of B in A are 2, 3
```

## Hint 1
Just traverse the A and if you found a divisor of B check whether it is prime or not.

## Solution approach

Just traverse the A and if you found a divisor of B check whether it is prime or not.

## Solution
### Editorial
```cpp
int Solution::solve(vector<int> &A, int B) {
    if(B<2)
    {
        return 0;
    }
    vector<bool>p(B+1);
    set<int>st;
    p[0]=p[1]=true;
    for(int i=2;i*i<=B;i++)
    {
        if(p[i]==false)
        {
            if(B%i==0)
            {
            st.insert(i);
            }
            for(int j=i*i;j<=B;j+=i)
            {
                if(p[j]==false)
                {
                    p[j]=true;
                }
            }
        }
    }
    for(int i=sqrt(B)+1;i<=B;i++)
    {
        if(p[i]==false && B%i==0)
        {
            st.insert(i);
        }
    }
    int c=0;
    for(int i=0;i<A.size();i++)
    {
        if(st.find(A[i])!=st.end())
        {
            c++;
        }
    }
    return c;
}
```

### Fastest
```cpp
int Solution::solve(vector<int> &A, int B) {
    if(B<2)
    {
        return 0;
    }
    vector<bool>p(B+1);
    set<int>st;
    p[0]=p[1]=true;
    for(int i=2;i*i<=B;i++)
    {
        if(p[i]==false)
        {
            if(B%i==0)
            {
            st.insert(i);
            }
            for(int j=i*i;j<=B;j+=i)
            {
                if(p[j]==false)
                {
                    p[j]=true;
                }
            }
        }
    }
    for(int i=sqrt(B)+1;i<=B;i++)
    {
        if(p[i]==false && B%i==0)
        {
            st.insert(i);
        }
    }
    int c=0;
    for(int i=0;i<A.size();i++)
    {
        if(st.find(A[i])!=st.end())
        {
            c++;
        }
    }
    return c;
}
```
### Lightweight
```cpp
bool isPrime(int N) {
    if (N <= 1) return false;
    if (N <= 3) return true;
    if (N % 2 == 0 || N % 3 == 0) return false;
    for (int i = 5; i * i <= N; i += 6) {
        if (N % i == 0 || N % (i + 2) == 0) {
            return false;
        }
    }
    return true;
}

int Solution::solve(vector<int> &A, int B) {
    int count = 0;
    for (int i = 0; i < A.size(); ++i) {
        if (isPrime(A[i]) && (B % A[i] == 0)) {
            count++;
        }
    }
    return count;
}

```

### Mine
```cpp
int Solution::solve(vector<int> &a, int n) {
    unordered_set<int> m;

    while (n % 2 == 0) {
        m.insert(2);
        n = n / 2;
    }

    for (int i = 3; i*i <= n; i = i + 2) {
        while (n % i == 0) {
            m.insert(i);
            n = n / i;
        }
    }

    if (n > 2)
        m.insert(n);

    int res = 0;
    for (int x : a) if (m.count(x)) res++;
    return res;
}
```
