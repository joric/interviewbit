# Maximum Frequency stack

https://www.interviewbit.com/problems/maximum-frequency-stack


You are given a matrix A which represent operations of size N x 2. Assume initially you have a stack-like data structure 
you have to perform operations on it.

Operations are of two types:

1. `1 x`: push an integer x onto the stack and return -1
2. `2 0`: remove and return the most frequent element in the stack.

If there is a tie for the most frequent element, the element closest to the top of the stack is removed and returned.

* A[i][0] describes the type of operation to be performed.
* A[i][1] describe the element x or 0 corresponding to the operation performed.


### Input Format

The only argument given is the integer array A.

### Output Format

Return the array of integers denoting answer to each operation.

### Constraints
```
1 <= N <= 100000
1 <= A[i][0] <= 2
0 <= A[i][1] <= 10^9
```
### For Example

```
Input 1:
    A = A = [
            [1, 5]
            [1, 7]
            [1, 5]
            [1, 7]
            [1, 4]
            [1, 5]
            [2, 0]
            [2, 0]
            [2, 0]
            [2, 0]  ]
Output 1:
    [-1, -1, -1, -1, -1, -1, 5, 7, 5, 4]

Input 2:
    A = A = [   
            [1, 5]
            [2 0]
            [1 4]   ]
Output 2:
    [-1, 5, -1]
```

## Solution

### Editorial
```cpp
vector<int> Solution::solve(vector<vector<int> > &A) {
    vector<int> ans;
    unordered_map <int, int> f;
    unordered_map <int, stack<int>> m;
    int maxfreq = 0;
    for (int i = 0; i < A.size(); i++)
    {
        int opt = A[i][0];
        int elem = A[i][1];
        if (opt == 1)
        {
            f[elem]++;
            maxfreq = max(maxfreq, f[elem]);
            m[f[elem]].push(elem);
            ans.push_back(-1);
        }
        else if (opt == 2)
        {
            int x = m[maxfreq].top();
            f[x]--;
            m[maxfreq].pop();
            if (m[maxfreq].size() == 0)     maxfreq--;
            ans.push_back(x);
        }
    }
    return ans;
}
```

### Fastest
```cpp
unordered_map<int, int> freq;
unordered_map<int, stack<int>> m;

void push(int x, int &maxfreq) {
    maxfreq = max(maxfreq, ++freq[x]);
    m[freq[x]].push(x);
}

int pop(int &maxfreq) {
    int x = m[maxfreq].top();
    m[maxfreq].pop();
    if (!m[freq[x]--].size()) maxfreq--;
    return x;
}

vector<int> Solution::solve(vector<vector<int> > &A) {
    vector<int> ans;
    freq.clear();
    m.clear();
    int maxfreq = 0;
    for(int i=0; i<A.size(); i++) {
        if(A[i][0] == 1) {
            ans.push_back(-1);
            push(A[i][1], maxfreq);
        } else {
            ans.push_back(pop(maxfreq));
        }
    }
    return ans;
}
```

### Lightweight
```cpp
vector<int> Solution::solve(vector<vector<int> > &A) {
    int n = A.size();
    vector<int> res(n, -1);
    unordered_map<int,int> freq;
    stack<int> st;
    for (int i = 0; i < n; ++i) {
        if (A[i][0]==1) {
            int x = A[i][1];
            st.push(x);
            ++freq[x];
            continue;
        }
        int mx = 0;
        for (auto& p : freq)mx=max(mx, p.second);
        stack<int> rev;
        while(freq[st.top()]!=mx)rev.push(st.top()),st.pop();
        int x = st.top();
        res[i]=x;
        st.pop();
        if(--freq[x]==0) freq.erase(x);
        while(!rev.empty()) st.push(rev.top()),rev.pop();
    }
    
    return res;
}
```

### Mine
```cpp
struct fstack {
    unordered_map<int,int> freq;
    unordered_map<int,stack<int>> m;
    int maxfreq = 0;
    
    void push(int x) {
        maxfreq = max(maxfreq, ++freq[x]);
        m[freq[x]].push(x);
    }
    
    int pop() {
        int x = m[maxfreq].top();
        m[maxfreq].pop();
        if (m[freq[x]--].empty()) maxfreq--;
        return x;
    }
};

vector<int> Solution::solve(vector<vector<int> > &A) {
    vector<int> res;
    fstack s;
    for (auto& v:A) {
        if (v[0]==1) {
            s.push(v[1]);
            res.push_back(-1);
        } else
            res.push_back(s.pop());
    }
    return res;
}
```
