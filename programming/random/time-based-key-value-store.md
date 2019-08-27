# Time Based Key-Value Store

https://www.interviewbit.com/problems/time-based-key-value-store/

You are required to perform N operations where each operation is of ei-ther type 1 or type 2.

You are given the following input:

An array of integers A that describes the type of i-th operation.
An array of strings B that describes the key in the i-th operation.
An array of strings C that describes the value in the i-th operation.
An array of integers D that describes the timestamp in the i-th operation.

Operations are of the following form:

* Type 1: set(string key, string value, int timestamp)
```
Stores the key and value, along with the given timestamp, where
key = B[i], value = C[i],  timestamp = D[i].
After Performing type 1 operation return "null".
```
* Type 2: get(string key, int timestamp), where 
```
key = B[i], 
timestamp = D[i] .
```
```
Returns a value such that set(key, value, timestamp_prev) was called previously, with timestamp_prev <= timestamp.
If there are multiple such values, it returns the one with the largest timestamp_prev.
If there are no values, it returns "null".
```

A[i] determines the type of operation if it is equal to 1 then you have to perform type 1 operations 
else you have to perform type 2 operation.

Return an array of the strings after performing all N operations.

### Input Format

```
The first argument given is the integer array A.
The second argument given is the string array B.
The third argument given is the string array C.
The fourth argument given is the integer array D.
```

### Output Format

Return an array of strings denoting answer for each operation.

### Constraints

```
1 <= N <= 100000
1 <= A[i] <= 2
1 <= |B[i]|, |C[i]| <= 100
1 <= D[i] <= 10^7
```

### For Example
```
Input 1:
    A = [1, 2, 2, 1, 2, 2]
    B = ["foo", "foo", "foo", "foo", "foo", "foo"]
    C = ["bar", "null", "null", "bar2", "null", "null"]
    D = [1, 1, 3, 4, 4, 5]
Output 1:
    ["null", "bar", "bar", "null", "bar2", "bar2"]
    Explanation 1:
        Operation 1: set("foo", "bar", 1) returns "null".
        Operation 2: get("foo", 1) returns "bar" as it has maximum value <= 1 with key as "foo".
        Operation 3: get("foo", 3) returns "bar" as it has maximum value <= 3 with key as "foo".
        Operation 4: set("foo", "bar2", 4) returns "null".
        Operation 5: get("foo", 4) returns "bar2" as it has maximum value <= 4 with key as "foo".
        Operation 6: get("foo", 5) returns "bar2" as it has maximum value <= 5 with key as "foo".
    
    

Input 2:
    A = [1, 1, 2, 2, 2, 2, 2]
    B = ["love", "love", "love", "love", "love", "love", "love"]
    C = ["high", "low", "null", "null", "null", "null", "null"]
    D = [10, 20, 5, 10, 15, 20, 25]
Output 2:
    ["null", "null", "null", "high", "high", "low", "low"]
```

## Solution
### Editorial
```cpp
unordered_map<string,vector<int>> mytable;
unordered_map<string,unordered_map<int,string>> subtable;


int b_s(vector<int>&v , int target){
    int left = 0, right = v.size()-1;
    while(right >= left){
        int mid = (right+left)/2;
        if(v[mid] == target)
            return mid;
        else if(v[mid] > target)
            right = mid-1;
        else
            left = mid+1;
    }
    return right;
}

void binary_insert(vector<int>& v, int target){
    if(v.size() == 0 || target > v[v.size()-1])
        v.push_back(target);
    else if(target < v[0])
        v.insert(v.begin(),target);
    else
        v.insert(v.begin()+b_s(v,target),target);
}

void sett(string &key, string &value, int timestamp) {
    binary_insert(mytable[key],timestamp);
    subtable[key][timestamp] = value;
}


string get(string key, int timestamp) {
    if(mytable[key].size() == 0 || timestamp < mytable[key][0])
            return "null";
    else{
        int idx = b_s(mytable[key],timestamp);
        return subtable[key][mytable[key][idx]];
    }
}


vector<string> solveit(vector<int> &type,vector<string> &key,vector<string> &value,vector<int> &timestamp){
    int n=type.size();
    mytable.clear();
    subtable.clear();
    vector<string> ans;
    for(int i=0; i<n; ++i){
        if(type[i]==1){
            sett(key[i],value[i],timestamp[i]);
            ans.push_back("null");
            }
        else
            ans.push_back(get(key[i],timestamp[i]));
    }
    return ans;
}

vector<string> Solution::solve(vector<int> &A, vector<string> &B, vector<string> &C, vector<int> &D) {
    return solveit(A,B,C,D);
}
```

### Fastest
```cpp
vector<string> Solution::solve(vector<int> &A, vector<string> &B,
vector<string> &C, vector<int> &D) {
    unordered_map<string, map<int,string>> kv;
    vector<string> res(A.size(), "null");
    for (int i = 0; i < A.size(); ++i) {
        if (A[i]==1)kv[B[i]][D[i]]=C[i];
        else {
            auto& v = kv[B[i]];
            auto it = v.upper_bound(D[i]);
            if (it!=v.begin()) {
                res[i] = std::prev(it)->second;
            }
        }
    }
    return res;
}
```

### Lightweight
```cpp
vector<string> Solution::solve(vector<int> &A, vector<string> &B, vector<string> &C, vector<int> &D) 
{
    vector<string> res(A.size(),"null");
    map<string,vector<pair<string,int>>> hm;
    
    for(int i=0;i<A.size();i++)
    {
        if(A[i]==1)
        {
                auto itr=hm.find(B[i]);
                if(itr==hm.end())
                {
                    vector<pair<string,int>> vec;
                    vec.push_back({C[i],D[i]});
                    hm.insert({B[i],vec});
                }else
                {
                    itr->second.push_back({C[i],D[i]});
                    
                }
                    
        }
        else if(A[i]==2)
        {
                auto itr=hm.find(B[i]);
              //  cout<<"itr "<<B[i]<<endl;
                if(itr!=hm.end())
                {
                   // cout<<"itr->first "<<itr->first<<endl;
                    auto vec=itr->second;
                    int max=0;
                    for(int j=0;j<vec.size();j++)
                    {
                       if(vec[j].second<=D[i]&&D[i]>=max)
                       {
                           res[i]=vec[j].first;
                           max=D[i];
                       }
                    }
                      
                }
        }
    }
   
    return res;
}
```

### Mine
```cpp
class TimeMap {
private:
    unordered_map<string, vector<int>> t;
    unordered_map<int, string> v;
    
public:
    void set(string key, string value, int timestamp) {
        t[key].emplace_back(timestamp);
        v[timestamp] = value;
    }
    
    string get(string key, int timestamp) {
        if(t.find(key) == end(t)) return "";
        auto ub = upper_bound(begin(t[key]), end(t[key]), timestamp);
        if(ub != begin(t[key])) return v[*(--ub)];
        return "";
    }
};

vector<string> Solution::solve(vector<int> &A, vector<string> &B, vector<string> &C, vector<int> &D) {
    vector<string> res;
    TimeMap m;
    for (int i=0; i<A.size(); i++) {
        string s;
        switch(A[i]) {
            case 1: m.set(B[i], C[i], D[i]); break;
            case 2: s = m.get(B[i], D[i]); break;
        }
        res.push_back(s=="" ? "null" : s);
    }
    return res;
}

```
