# Simple Queries

https://www.interviewbit.com/problems/simple-queries/

You are given an array A having N integers.

You have to perform the following steps in a given order.

1. generate all subarrays of A.
2. take the maximum element from each subarray of A and insert it into a new array G.
3. replace every element of G with the product of their divisors mod 1e9 + 7.
4. sort G in descending order
5. perform Q queries

In each query, you are given an integer K, where you have to find the Kth element in G.

Note: Your solution will run on multiple test cases so do clear global variables after using them.

### Input Format
```
The first argument given is an Array A, having N integers.
The second argument given is an Array B, where B[i] is the ith query.
```
### Output Format

Return an Array X, where X[i] will have the answer for the ith query.

### Constraints
```
1 <= N <= 1e5
1 <= A[i] <= 1e5
1 <= Q <= 1e5
1 <= k <= (N * (N + 1))/2 
```
### For Example
```
Input:
    A = [1, 2, 4]
    B = [1, 2, 3, 4, 5, 6]
Output:
    X = [8, 8, 8, 2, 2, 1]
   
Explanation:
    subarrays of A	  maximum element
    ------------------------------------
    1. [1]							1
    2. [1, 2]						2
    3. [1, 2, 4]					4
    4. [2]							2
    5. [2, 4]						4
    6. [4]							4

	original
	G = [1, 2, 4, 2, 4, 4]
	
	after changing every element of G with product of their divisors
	G = [1, 2, 8, 2, 8, 8]
	
	after sorting G in descending order
	G = [8, 8, 8, 2, 2, 1]
```

## Hint

A brute force solution to solve this problem is to do as instructed in the statement.

But this will give time out.

So can we reduce complexity?

Can we do Binary Search for each query?

do you know product of divisors of a number can be written as N D/2, where N is number and D is number of divisors of N.

## Solution approach

We can solve this problem by doing the binary search for each query.

How?

First, you need to find that how many times an element will appear in array G. i.e in how many subarrays an element is the greatest one.

You can find that by finding the next greater element for the current element in both sides and then by multiplying them.

Once you found the frequency of each element in an array G, you can sort the pairs(product_of_divisors_of_element, frequency) according to there value in descending order followed by taking the prefix sum of there frequencies you can do the binary search for each query.

Please refer complete solution for more insight.


## Solution
### Editorial
```cpp
#define ll long long int
const int mn = 1e5 + 5;
const ll mod = 1e9 + 7;
ll power(ll a, ll g) {ll ag = 1; while(g){if(g&1) ag = (ag%mod * a%mod)%mod; a = (a%mod * a%mod)%mod; g >>= 1;} return ag;}

ll p[mn];

void pre_compute_product_of_divisors() {
    p[0] = 0; p[1] = 1;
    if(p[2] != 0) return;
    for(ll i = 2; i < mn; i += 1) {
        if(p[i] == 0) {
            p[i] = 2;
            for(ll j = i+i; j < mn; j += i) {
                if(p[j] == 0) p[j] = 1;
                ll tmp = j;
                ll cnt = 0;
                while(tmp % i == 0) {
                    cnt += 1;
                    tmp /= i;
                }
                p[j] *= (cnt + 1);
            }
        }
    }
    for(int i = 2; i < mn; i += 1) {
        p[i] = (power(i, p[i]/2)%mod * (p[i]&1 ? (ll)sqrt(i) : 1)%mod)%mod;
    }
}

// comparator to sort in descending order
bool compare(pair<int, long long int> a, pair<int, long long int> g) {
    if(a.first == g.first)
        return a.second < g.second;
    else
        return a.first > g.first;
}

vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    
    pre_compute_product_of_divisors();
    
    int n = (int)A.size();
    // create arrays to store length of longest segment in which ith element is greater
    long long int l[n], r[n], lr[n];
    // initialize elements array equal to 1.
    for(int i = 0; i < n; i += 1) {
        l[i] = r[i] = 1;
    }
    // find next greater element to the left of the current element
    for(int i = 1; i < n; i += 1) {
        int last = i-1;
        while(last >= 0 and A[i] > A[last]) {
            l[i] += l[last];
            last -= l[last];
        }
    }
    // find next greater element to the right of the current element
    for(int i = n-2; i >= 0; i -= 1) {
        int last = i+1;
        while(last < n and A[i] >= A[last]) {
            r[i] += r[last];
            last += r[last];
        }
    }
    // The number of subarrays in which current element will be the greater
    for(int i = 0; i < n; i += 1) {
        lr[i] = l[i] * r[i];
    }
    // Sort elements in descending order according to there value
    pair<int, long long int> ag[n];
    for(int i = 0; i < n; i += 1) {
        ag[i] = {p[A[i]], lr[i]};
    }
    sort(ag, ag + n, compare);

    // Take Prefix Sum of frequencies of elements
    long long pre[n];
    pre[0] = ag[0].second;
    for(int i = 1; i < n; i += 1) {
        pre[i] = pre[i-1] + ag[i].second;
    }
    
    // do Binary search for each query
    int q = (int)B.size();
    vector<int> ans(q);
    for(int i = 0; i < q; i += 1) {
        auto id = lower_bound(pre, pre + n, B[i]) - pre;
        ans[i] = ag[id].first;
    }
    // return the ans array
    return ans;
}
```

### Lightweight
```cpp
vector<int> Solution::solve(vector<int> &a, vector<int> &b) {
    int n=a.size();
    stack<int >s;
    //map<int ,int>hm;
    s.push(0);
    vector<int >v(n,0);
    for(int i=1;i<n;i++)
    {
        if(a[i]<=a[s.top()])
            s.push(i);
        else
        {
            while(!s.empty() && a[i]>a[s.top()])
            {
                v[s.top()]+=i-s.top();
               // cout<<i<<" "<<hm[a[s.top()]]<<endl;
                s.pop();
            }
            s.push(i);
        }
    }
    while(!s.empty())
    {
        v[s.top()]+=n-s.top();
        s.pop();
    }
    s.push(n-1);
    for(int i=n-2;i>=0;i--)
    {
        if(a[i]<a[s.top()])
            s.push(i);
        else
        {
            while(!s.empty() && a[i]>=a[s.top()])
            {
                int tmp=v[s.top()]-1;
                tmp=(tmp)*(s.top()-i-1);
                v[s.top()]+=(s.top()-i-1+tmp);
                
                //cout<<"check"<<i<<" "<<v[s.top()]<<endl;
                s.pop();
            }
            s.push(i);
        }
    }
    while(!s.empty())
    {
        int tmp=v[s.top()]-1;
        tmp=(tmp)*(s.top());
        v[s.top()]+=(s.top()+tmp);
        s.pop();
    }
    for(int i=0;i<n;i++)
    {
        int tmp=a[i];
        long long int  dv=1;
        for(int k=1;k*k<=tmp;k++)
        {
            
            if(k*k==tmp)
            {
                dv=(dv*k)%1000000007;
            }
            else if(tmp%k==0)
            {
                dv=(dv*k)%1000000007;
                dv=(dv*(tmp/k))%1000000007;
            }
        }
        a[i]=dv;
        //cout<<" div "<<a[i]<<endl;
    }
    vector<pair<int ,int>>av(n);
    for(int i=0;i<n;i++)
    {
        av[i].first=a[i];
        av[i].second=v[i];
    }
    sort(av.begin(),av.end(),greater<pair<int,int>>());
    //for(int i=0;i<n;i++)
    //    cout<<av[i].first<<" "<<av[i].second<<endl;
    int m=b.size();
    vector<pair<int ,int>>bi(m);
    for(int i=0;i<m;i++)
    {
        bi[i].first=b[i];
        bi[i].second=i;
    }
    sort(bi.begin(),bi.end());
    vector<int>ans(m);
    int j=0,i=0,sm=0;
    while(j<n && i<m)
    {
        //cout<<bi[i].second<<" "<<bi[i].first<<" "<<sm<<" check"<<endl;
        
        if(sm<bi[i].first)
        {
            sm+=av[j].second;
            j++;
        }
        if(bi[i].first<=sm)
        {
            ans[bi[i].second]=av[j-1].first;
            i++;
        }
        
    }
    return ans;
    
    
}
```



