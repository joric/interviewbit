# Find The Next

https://www.interviewbit.com/problems/find-the-next

Given an array of integers A of size N. You are given another array of integers B of size Q representing queries.

For ith query, you need to find an integer Z strictly greater than B[i] 
such that Z is not present in the array A. You need to minimize the value of Z.

Return an array of size Q denoting answer for each query.

### Input Format

The first argument given is the integer array A.
The second argument given is the integer array B.

### Output Format

Return an array of integers denoting answer for each query.

### Constraints

```
1 <= N, Q <= 100000
1 <= A[i], B[i] <= 10^9 
```

### For Example

```
Input 1:
    A = [1, 5, 10, 2, 7]
    B = [1, 6, 9, 12, 11]
Output 1:
    [3, 8, 11, 13, 12]

Input 2:
    A = [7, 14, 9, 6, 11, 12, 14]
    B = [18, 14, 2, 18, 16, 2, 10]
Output 2:
    [19, 15, 3, 19, 17, 3, 13] 
```
## Solution

### Editorial
```cpp
vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    int n=A.size();
    int m=B.size();
    vector<int> ans(m);
    pair<int,int> b[m];
    for(int i=0; i<m; ++i){
        b[i].first=B[i];
        b[i].second=i;
    }
    sort(A.begin(),A.end());
    sort(b,b+m);
    int x=b[0].first+1;
    int j=0,i=0;
    while(i<m){
        while(j<n&&x>A[j])
            ++j;
        if(j<n&&x==A[j]){
            ++x;
            continue;
        }
        while(i<m&&x>b[i].first){
            ans[b[i].second]=x;
            ++i;
        }
        if(i<m)
            x=b[i].first+1;
    }
    return ans;
}
```

### Fastest
```cpp
vector<int> anotherapproach(vector<int> &A,vector<int> &B){
    int n=A.size();
    int q=B.size();
    sort(A.begin(),A.end());
    map<int,int> mp;
    for(int i=0;i<n;){
        int j=i;
        for(;j<n-1;j++)
            if(A[j]<A[j+1]-1)
                break;
        for(int k=i;k<=j;k++)
            mp[A[k]]=A[j];
        i=j+1;
    }
    vector<int> ans;
    for(auto &x:B){
        if(mp[x]==0){
            if(mp[x+1]==0)
                ans.push_back(x+1);
            else
                ans.push_back(mp[x+1]+1);
        }
        else
            ans.push_back(mp[x]+1);
    }
    return ans;
}


vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    int n=A.size();
    int m=B.size();
    vector<int> ans(m);
    pair<int,int> b[m];
    for(int i=0; i<m; ++i){
        b[i].first=B[i];
        b[i].second=i;
    }
    sort(A.begin(),A.end());
    sort(b,b+m);
    int x=b[0].first+1;
    int j=0,i=0;
    while(i<m){
        while(j<n&&x>A[j])
            ++j;
        if(j<n&&x==A[j]){
            ++x;
            continue;
        }
        while(i<m&&x>b[i].first){
            ans[b[i].second]=x;
            ++i;
        }
        if(i<m)
            x=b[i].first+1;
    }
    return ans;
}
```

### Lightweight
```cpp
vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    vector<int> res;
    sort(A.begin(), A.end());
    for (int i=0; i<B.size(); i++) {
        int x = B[i]+1;
        while (true) {
            //cout << "i:"<<i<<" B[i]="<< B[i] << " testing "<<x<<endl;
            auto it = lower_bound(A.begin(), A.end(), x);
            if (it==A.end() || *it!=x) {
                res.push_back(x);
                break;
            } else {
                //cout << "bound: " << *it << endl;
            }
            x++;
        }
    }
    return res;
}
```

### Mine (Lightweight autofills with mine??)
```cpp
vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    vector<int> res;
    sort(A.begin(), A.end());
    for (int i=0; i<B.size(); i++) {
        int x = B[i]+1;
        while (true) {
            //cout << "i:"<<i<<" B[i]="<< B[i] << " testing "<<x<<endl;
            auto it = lower_bound(A.begin(), A.end(), x);
            if (it==A.end() || *it!=x) {
                res.push_back(x);
                break;
            } else {
                //cout << "bound: " << *it << endl;
            }
            x++;
        }
    }
    return res;
}
```
### Another Mine
```cpp
vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    int n = A.size(), m = B.size(), i = 0, j = 0;
    vector<int> res(m);
    vector<int> idx(m);
    sort(A.begin(), A.end());
    iota(idx.begin(), idx.end(), 0);
    sort(idx.begin(), idx.end(), [&B](int i, int j) { return B[i] < B[j]; });
    int x = B[idx[i]] + 1;
    while (i < m) {
        while (j < n && x > A[j]) j++;
        if (j < n && x == A[j]) {
            x++;
            continue;
        }
        while (i < m && x > B[idx[i]]) res[idx[i++]] = x;
        if (i < m) x = B[idx[i]] + 1;
    }
    return res;
}
```
