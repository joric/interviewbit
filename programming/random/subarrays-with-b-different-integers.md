# Subarrays with B Different Integers

https://www.interviewbit.com/problems/subarrays-with-b-different-integers/

Given an array of integers A and an integer B. Find and return the number of good subarrays of A.

A subarray of A is said to be good if the number of different integers in that subarray is exactly B.


### Input Format

The first argument given is the integer array A.

The second argument given is an integer B.

### Output Format

Return the number of good subarrays of A.

### Constraints
```
1 <= length of the array <= 40000
1 <= A[i] <= length of the array
1 <= B <= length of the array
```
### For Example

Input 1:
    A = [1, 2, 1, 2, 3]
    B = 2
Output 1:
    7
    Explanation 1:
     Subarrays having exacty 2 different elements are: 
        [1, 2], [2, 1], [1, 2], [2, 3], [1, 2, 1], [2, 1, 2], [1, 2, 1, 2].

Input 2:
    A = [1, 2, 1, 3, 4]
    B = 3
Output 2:
    3


## Solution
### Editorial
```cpp
unordered_map<int,int> mp;

void add(int x){
    ++mp[x];
}

void removee(int x){
    --mp[x];
    if(mp[x]==0)
        mp.erase(x);
}

int solveit(vector<int>& A, int K) {
    mp.clear();
    int n=A.size();
    if(n<K)
        return 0;
    int l = 0, r = 0, d = 0, ans = 0;
    while (r <= n) {
        if (r < n && mp.size() < K)
            add(A[r++]);
        else if(mp.size()==K) {
            if(d<=r){
                d = r;
                while(d < n && mp.find(A[d]) != mp.end())
                    d++;
            }
            while(mp.size()==K){
                ans+=(d+1-r);
                removee(A[l++]);
            }
        }
        else
            r++;
    }
    return ans;
}


int Solution::solve(vector<int> &A, int B) {
    return solveit(A,B);
}
```

### Fastest
```cpp
unordered_map<int,int> mp;

void add(int x){
    ++mp[x];
}

void removee(int x){
    --mp[x];
    if(mp[x]==0)
        mp.erase(x);
}

int solveit(vector<int>& A, int K) {
    mp.clear();
    int n=A.size();
    if(n<K)
        return 0;
    int l = 0, r = 0, d = 0, ans = 0;
    while (r <= n) {
        if (r < n && mp.size() < K)
            add(A[r++]);
        else if(mp.size()==K) {
            if(d<=r){
                d = r;
                while(d < n && mp.find(A[d]) != mp.end())
                    d++;
            }
            while(mp.size()==K){
                ans+=(d+1-r);
                removee(A[l++]);
            }
        }
        else
            r++;
    }
    return ans;
}


int Solution::solve(vector<int> &A, int B) {
    return solveit(A,B);
}
```

### Lightweigth
```cpp
int Solution::solve(vector<int> &A, int B) 
{
    int n=A.size();
    map<int,int> m;
    int count=0;
    int l=0;
    int r=0;
    int d=0;
    while(r<=n)
    {
        if(r<n && m.size()<B)
        {
            m[A[r]]++;
            r++;
        }
        else if(m.size()==B)
        {
            if(d<=r)
            {
                d=r;
                while(d<n && m.find(A[d])!=m.end())
                {
                    d++;
                }
            }
            while(m.size()==B)
            {
                count+=(d+1-r);
                m[A[l]]--;
                if(m[A[l]]==0)
                {
                    m.erase(A[l]);
                }
                l++;
            }
        }
        else
        {
            r++;
        }
    }
    
    return count;
}
```

