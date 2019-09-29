# Count Right Triangles

https://www.interviewbit.com/problems/count-right-triangles/

Given two arrays of integers A and B of size N each, where each pair (A[i], B[i]) for 0 <= i < N
represents a unique point (x, y) in 2D Cartesian plane.

Find and return the number of unordered triplets (i, j, k) such that (A[i], B[i]), (A[j], B[j]) and (A[k], B[k])
form a right angled triangle with the triangle having one side parallel to the x-axis and one side parallel to the y-axis.

Note: The answer may be large so return the answer modulo (10^9 + 7).

### Input Format

* The first argument given is an integer array A.
* The second argument given is the integer array B.

### Output Format

Return the number of unordered triplets that form a right angled triangle modulo (10^9 + 7).

### Constraints

* 1 <= N <= 100000
* 0 <= A[i], B[i] <= 10^9 

### For Example
```
Input 1:
    A = [1, 1, 2]
    B = [1, 2, 1]
Output 1:
    1

Input 2:
    A = [1, 1, 2, 3, 3]
    B = [1, 2, 1, 2, 1]
Output 2:
    6
```

## Hint1
Try fixing each point as the intersection of perpendicular and base and try finding pther points.

## Solution Approach

Try fixing each point as the intersection of perpendicular and base and try finding other points.

Once it is fixed, for the other two points, one point will share the same x-coordinate and the other point will share the same 
y-coordinate with the selected point.

For points sharing same x or y coordinate we can use map to store the points.

## Solution
### Editorial
```cpp
int Solution::solve(vector<int> &a, vector<int> &b) {
    int n = a.size();
    map<int, int> mp[2];

    for(int i = 0; i < n; i++) {
        mp[0][a[i]]++;
        mp[1][b[i]]++;
    }

    long ans = 0, mod = (long)(1e9 + 7);
    for(int i = 0; i < n; i++) {
        ans = (ans + 1L * (mp[0][a[i]] - 1L) * (mp[1][b[i]] - 1L) ) % mod;
    }
    return (int)ans;
}
```

### Fastest
```cpp
int Solution::solve(vector<int> &A, vector<int> &B) {
    int n = A.size(), res = 0, mod = 1e9+7;
    unordered_map<int, int> x_cnt, y_cnt;
    for (int x : A)++x_cnt[x];
    for (int y : B)++y_cnt[y];
    
    for (int i = 0; i < n; ++i) {
        int x = A[i], y = B[i];
        long nx = x_cnt[x] - 1, ny = y_cnt[y] - 1;
        res = (res + nx*ny)%mod;
    }
    
    return res;
}
```

### Lightweight
```cpp
#define pb push_back
#define mpr make_pair
#define ff first
#define ss second
int Solution::solve(vector<int> &a, vector<int> &b) {

    int n=a.size();
    vector<pair<int,int>> v;
    map<int,int> ymp;
    for (int i = 0; i < n; ++i)
    {
        v.pb(mpr(a[i],b[i]));
        ymp[b[i]]++;
    }
    int ans=0,x,y,j;
    sort(v.begin(), v.end());
    int count;
    for (int i = 0; i < n; ++i)
    {
        count=0;
        x=v[i].ff;
        j=i+1;
        while(j<n && v[j].ff==x)
        {
            j++;
            count++;
        }
        j=i-1;
        while(j>=0 && v[j].ff==x)
        {
            count++;
            j--;
        }
        y=ymp[v[i].ss];
        y--;
        ans+=count*y;
    }

    return ans;
}
```

