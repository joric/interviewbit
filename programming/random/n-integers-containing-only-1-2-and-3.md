# N integers containing only 1,2 and 3

https://www.interviewbit.com/problems/n-integers-containing-only-1-2-and-3/

Given an integer A. Find and Return first positive A integers in ascending order containing only digits 1,2 and 3.

### NOTE

all the A integers will fit in 32 bit integer.

### Input Format

The only argument given is integer A.
### Output Format

Find and Return first positive A integers in ascending order containing only digits 1,2 and 3.
### Constraints
```
1 <= A <= 29500
```
### For Example

```
Input 1:
    A = 3
Output 1:
    [1, 2, 3]

Input 2:
    A = 7
Output 2:
    [1, 2, 3, 11, 12, 13, 21]
```
## Solution
### Editorial
```cpp
vector<int> Solution::solve(int A) {
    vector<int> v={1,2,3};
    
    for(int i=0;i<=A;i++){
        for(int j=0;j<=2;j++){
            v.push_back(v[i]*10+v[j]);
        }
    }
    v.erase(v.begin()+A,v.end());
    return v;
}
```
### Fastest
```cpp
vector<int> doit(int n) {
    vector<int> ans;
    queue<int> q;
    q.push(1);
    q.push(2);
    q.push(3);
    int mx = 1000000000;
    while (ans.size() < n) {
        int x = q.front();
        q.pop();
        ans.push_back(x);
        int x1 = x * 10 + 1;
        int x2 = x * 10 + 2;
        int x3 = x * 10 + 3;
        if (x1 < mx)
            q.push(x1);
        if (x2 < mx)
            q.push(x2);
        if (x3 < mx)
            q.push(x3);
    }
    return ans;
}

vector<int> Solution::solve(int A) {
    return doit(A);
}

```

### Lightweight
```cpp
vector<int> doit(int n) {
    vector<int> ans;
    queue<int> q;
    q.push(1);
    q.push(2);
    q.push(3);
    int mx = 1000000000;
    while (ans.size() < n) {
        int x = q.front();
        q.pop();
        ans.push_back(x);
        int x1 = x * 10 + 1;
        int x2 = x * 10 + 2;
        int x3 = x * 10 + 3;
        if (x1 < mx)
            q.push(x1);
        if (x2 < mx)
            q.push(x2);
        if (x3 < mx)
            q.push(x3);
    }
    return ans;
}

vector<int> Solution::solve(int A) {
    return doit(A);
}

```

### Mine
```cpp
// taken from "megaprime numbers"
long next(long x) {
    int a=1, b=2, c=3;
    if (x == 0) return 0;
    long d = x / 10;
    long d1 = next(d + (x % 10 > c ? a : 0));
    if (d != d1) return 10 * d1 + a;
    return d1 * 10 + (x % 10 <= a ? a : x % 10 <= b ? b : c);
}
vector<int> Solution::solve(int A) {
    vector<int> res;
    long k = 1;
    for (int i=0; i<A; i++) {
        res.push_back(k);
        k = next(k+1);
    }
    return res;
}
```

