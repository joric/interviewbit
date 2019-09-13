# Falling Dominoes

https://www.interviewbit.com/problems/falling-dominoes/

Given two array of integers A and B of size N.

Ai and Bi represents the position of a domino on x axis and its height.

For every Domino, You are required to find out out how many dominoes will fall if you pushes it to the right.

Consider that a domino falls if it is touched strictly above the base.

In other words, the fall of the domino with the initial coordinate Ai and height Bi
leads to the fall of all dominoes on the segment [Ai, Ai+Bi-1].

Return an array of integer C of size N such that

Ci: the number of dominoes that will fall
if you pushes the ith domino to the right (including the domino itself).

Note: All Ai are distinct.

### Input Format

The argument given are two integer arrays A and B.

### Output Format

Return an array of integers C.

### Constraints

1 <= N <= 100000

-10^8 <= A[i] <= 10^8

2 <= B[i] <= 10^8

### For Example
```
Input 1:
    A = [-13, 10, -16, 2] 
    B = [10, 7, 7, 11]
Output 1:
    C = [1, 1, 2, 2]

Input 2:
    A =  [-12, -6, -2, 3, -7, 13, -14, 19, -10, -18, 11] 
    B =  [6, 5, 10, 2, 3, 10, 5, 8, 6, 5, 9] 
Output 2:
    C = [6, 3, 2, 1, 4, 2, 7, 1, 5, 8, 3]
```

## Hint 1
let F (i) be the number of the most right domino, which will fall if you push the i-th domino.

Try to find F(i) optimally.

## Solution Approach

sort all the dominoes by the A[i] coordinate from left to right,
remembering the initial numbering. This numbering is only required to return the answer in the correct order.

1. If you push any domino to the right, then among the fallen dominoes everyone (except the domino) will be to the right of the one that was pushed.
2. If any domino fell, then all the dominoes that are to the left of this, but not to the left of the one that was initially pushed, also fell.
3. For each domino, you can determine the number of the most right dominoes such that it falls if you push this one

Let F (i) be the number of the most right domino, which will fall if you push the i-th domino.

By itself, the i-th domino may not fall on F(i) -th domnio, but F (i) will still fall.

If the domino in the fall does not touch any other domino, then F (i) = i.

Obviously, such an equality holds for the right-right domino.

For the remaining dominoes, one can find the value of F (i) using the already found F (j), where j> i.

For each domino number i, one can find such a domino number j, that if the i-th falls,
it will bring down the j-th (directly), and this j will be maximal.

That is, to find the rightmost of the dominoes, which the i-th will tumble down directly.

Further, F (i) = max (F (k)), where k takes values ​​in the interval [i + 1, j].

The implementation of the solution with the asymptotics O (N 2 ) does not exactly fit into the time constraints.

Therefore, it is necessary to write a solution for at least O (NlogN),

for which it suffices to use the data structure of the interval tree type.

In short, solve the problem of Range Maximum Query.

## Solution
### Editorial
```cpp
const int N =100005;
int ctr=0;
int st[4*N], st2[4*N], ans[N];
map<int, int> m, rm;
map<int, int> idx;
pair<int, int> a[N];
vector<int> v;

void update(int node, int L, int R, int pos, int val){
    if(L==R){
        st[node]=val;
        st2[node]=L;
        return;
    }
    int M=(L+R)/2;
    if(pos<=M)
        update(node*2, L, M, pos, val);
    else
        update(node*2+1, M+1, R, pos, val);
    st[node]=max(st[node*2], st[node*2+1]);
    if(st[node]==st[node*2])
        st2[node]=st2[node*2];
    else
        st2[node]=st2[node*2+1];
}

pair<int, int> query(int node, int L, int R, int i, int j){
    if(j<L || i>R)
        return {-1e9, 0};
    if(i<=L && R<=j)
        return {st[node], st2[node]};
    int M=(L+R)/2;
    pair<int, int> left=query(node*2, L, M, i, j);
    pair<int, int> right=query(node*2+1, M+1, R, i, j);
    if(left.first>right.first)
        return left;
    return right;
}

void clean(int n){
    v.clear();
    idx.clear();
    m.clear();
    rm.clear();
    for(int i=0; i<=4*n; ++i){
        st2[i]=0;
        st[i]=0;
        if(i<=n)
            ans[i]=0;
    }
    ctr=0;
}

vector<int> doit(vector<int> &x,vector<int> &h){
    int n=x.size();
    clean(n);
    for(int i=1; i<=n; ++i){
        a[i].first=x[i-1];
        a[i].second=h[i-1];
        idx[a[i].first]=i;
        m[a[i].first];
        v.push_back(a[i].first);
    }
    sort(v.begin(),v.end());
    for(auto &it:m)
        it.second=++ctr;
    sort(a+1,a+n+1);
    vector<int> anss;
    for(int i=n; i>=1; --i){
        int cur=a[i].first;
        int nxt=a[i].first+a[i].second;
        auto it=lower_bound(v.begin(), v.end(), nxt);
        it--;
        int maxidx=m[*it];
        pair<int, int> p = query(1, 1, n, i, maxidx);
        int curans;
        if(p.first==0)
            curans=1;
        else
            curans=p.first - i;
        update(1, 1, n, i, i+curans);
        ans[idx[a[i].first]]=curans;
    }
    for(int i=1;i<=n;i++)
        anss.push_back(ans[i]);
    return anss;
}


vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    return doit(A,B);
}
```

### Fastest
```cpp
struct jd
{
    int pia;  //position in array
    int pix;  //position in x-axis
    int r;    //range to right side
};
bool comp(jd a,jd b)
{
    return a.pix<b.pix;
}
vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    int n=A.size();
    jd arr[n];
    vector<int>ans(n);
    for(int i=0;i<n;i++)
    {
        arr[i].pix=A[i];
        arr[i].pia=i;
        arr[i].r=A[i]+B[i]-1;
    }
    sort(arr,arr+n,comp);
    stack<int>s;
    for(int i=n-1;i>=0;i--)
    {
        ans[arr[i].pia]=1;
        while(!s.empty() and arr[i].r>=arr[s.top()].pix)
        {
             ans[arr[i].pia]+= ans[arr[s.top()].pia];
             s.pop();
        }
        s.push(i);
    }
    return ans;
}
```

### Lightweight
```cpp
struct point{
    int s,f,ind;
};
bool compare(point a,point b){
    return a.s<b.s;
}
vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    int n = A.size();
    vector<int> ans(n);
    vector<point> v(n);
    for(int i=0;i<n;i++){
        v[i].s = A[i];
        v[i].f = A[i]+B[i]-1;
        v[i].ind = i;
    }
    sort(v.begin(),v.end(),compare);
    stack<int> s;
    for(int i=n-1;i>=0;i--){
        ans[v[i].ind] = 1;
        while(!s.empty()&&v[s.top()].s<=v[i].f){
            ans[v[i].ind] += ans[v[s.top()].ind];
            s.pop();
        }
        s.push(i);
    }
    return ans;
}
```

### Mine
```cpp
vector<int> Solution::solve(vector<int> &pos, vector<int> &height) {
    int n = pos.size();
    vector<int> idx(n), res(n);
    iota(idx.begin(), idx.end(), 0);
    sort(idx.begin(), idx.end(), [&pos](int a, int b){ return pos[a]<pos[b]; });
    stack<int> s;
    for (int i = n - 1; i >= 0; i--) {
        int j = idx[i];
        res[j] = 1;
        while (!s.empty() && pos[s.top()]<=pos[j]+height[j]-1) {
            res[j] += res[s.top()];
            s.pop();
        }
        s.push(j);
    }
    return res;
}
```

## References

* https://codeforces.com/problemset/problem/56/E
