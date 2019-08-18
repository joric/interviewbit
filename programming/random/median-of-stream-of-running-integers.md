# Median of stream of running integers

https://www.interviewbit.com/problems/median-of-stream-of-running-integers

Given an array of integers A denoting a stream of integers.
A new array of integer B is formed and C are formed. Each time an integer is encountered in a stream 
append it at the end of B and append median of array B at the C.

Find and return the array C.

### NOTE

1. If the number of elements are n in B and n is odd then consider medain as B[n/2] ( B must be in sorted order).

2. If the number of elements are n in B and n is even then consider medain as B[n/2-1] ( B must be in sorted order).


### Input Format

The only argument given is the integer array A.

### Output Format

Return the array C.

### Constraints
```
1 <= length of the array <= 100000
1 <= A[i] <= 1000000000
```
### For Example
```
Input 1:
    A = [1, 2, 3, 4, 5]
Output 1:
    C = [1, 1, 2, 2, 3]
    
    stream          median
    [1]             1
    [1, 2]          1
    [1, 2, 3]       2
    [1, 2, 3, 4]    2
    [1, 2, 3, 4, 5] 3

Input 2:
    A = [5, 17, 100, 11]
Output 2:
    C = [5, 5, 17, 11]
```

## Solution

### Editorial
```cpp
class MedianStream{
private:
    multiset<int> data;
    multiset<int>::iterator mid;
public:
    
    void init(){
        data.clear();
        mid = data.end();
    }
    
    void add(int num){
        const size_t n = data.size();
        data.insert(num);
        if(!n){ /// initially empty container
            mid = data.begin();
        }
        else if(num >= *mid){
            mid = n&1 ? mid : next(mid);
        }
        else {
            mid = n&1 ? prev(mid) : mid;
        }
    }
    
    int getMedian(){
        if(mid!=data.end()) return *mid;
        return 0;
    }
    
};

vector<int> Solution::solve(vector<int> &A){
    // priority_queue<int, vector<int>, greater<int>> pmin;
    // priority_queue<int> pmax;
    // vector<int> res;
    // for(int a: A){
        
    //     pmax.push(a);
    //     pmin.push(pmax.top()); pmax.pop();
    //     if(pmin.size() > pmax.size()){
    //         pmax.push(pmin.top()); pmin.pop();
    //     }
        
    //     res.push_back(pmax.top());
    // }
    // return res;
    /// runtime O(log n)
    
    /// another approach
    /// we can use self balancing BST
    /// root and one of it's child yeilds the median
    /// in c++, we can use multiset to avoid complexity
    
    MedianStream ms;
    ms.init();
    vector<int> res;
    for(int a: A){
        ms.add(a);
        res.push_back(ms.getMedian());
    }
    
    return res;
}
```

### Fastest
```cpp
vector<int> Solution::solve(vector<int> &arr) {
    vector<int> res;
    int n = arr.size();
    priority_queue<int> s;
    priority_queue<int, vector<int>, greater<int>> g;

    int med = arr[0];
    s.push(arr[0]);

    res.push_back(med);

    for (int i = 1; i < n; i++) {
        int x = arr[i];

        if (s.size() > g.size()) {
            if (x < med) {
                g.push(s.top());
                s.pop();
                s.push(x);
            } else 
                g.push(x);
                
            med = s.size()>g.size() ? g.top() : s.top();
        }

        else if (s.size() == g.size()) {
            if (x < med) {
                s.push(x);
                med = (int)s.top();
            } else {
                g.push(x);
                med = (int)g.top();
            }
        }

        else {
            if (x > med) {
                s.push(g.top());
                g.pop();
                g.push(x);
            } else
                s.push(x);
            med = s.size()>g.size() ? g.top() : s.top();
        }

        res.push_back(med);
    }
    return res;
}
```

### Lightweight
```cpp
vector<int> Solution::solve(vector<int> &A) {
    int n = A.size();
    vector<int> res(n);
    
    priority_queue<int> L, R;
    for (int i = 0; i < n; ++i) {
        L.push(A[i]);
        if(i&1)R.push(-L.top()),L.pop();
        if(!R.empty()&&L.top()>-R.top()) {
            int x = L.top(), y = -R.top();
            L.pop(), R.pop();
            L.push(y), R.push(-x);
        }
        res[i]=L.top();
    }
    return res;
}
```

### Mine
```cpp
vector<int> Solution::solve(vector<int> &arr) {
    vector<int> res;
    int n = arr.size();
    priority_queue<int> s;
    priority_queue<int, vector<int>, greater<int>> g;

    int med = arr[0];
    s.push(arr[0]);

    res.push_back(med);

    for (int i = 1; i < n; i++) {
        int x = arr[i];

        if (s.size() > g.size()) {
            if (x < med) {
                g.push(s.top());
                s.pop();
                s.push(x);
            } else 
                g.push(x);
                
            med = s.size()>g.size() ? g.top() : s.top();
        }

        else if (s.size() == g.size()) {
            if (x < med) {
                s.push(x);
                med = (int)s.top();
            } else {
                g.push(x);
                med = (int)g.top();
            }
        }

        else {
            if (x > med) {
                s.push(g.top());
                g.pop();
                g.push(x);
            } else
                s.push(x);
            med = s.size()>g.size() ? g.top() : s.top();
        }

        res.push_back(med);
    }
    return res;
}
```

```
