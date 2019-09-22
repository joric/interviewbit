# Task Scheduler

https://www.interviewbit.com/problems/task-scheduler/


Given a string A representing tasks CPU need to do it. A consists of uppercase English letters where different letters represent different tasks. Tasks could be done without the original order.
Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval B that means between two same tasks,
there must be at least B intervals that CPU is doing different tasks or just be idle.

Find the least number of intervals the CPU will take to finish all the given tasks.

### Input Format

The first argument given is string A.

The second argument given is an integer B.

### Output Format

Return the least number of intervals the CPU will take to finish all the given tasks.

### Constraints

```
1 <= |A| <= 10000
0 <= B <= 100 
```

### For Example

```
Input 1:
    A = "AAABBB"
    B = 2
Output 1:
    8
    Explanation 1:
        CPU can finish the tasks in one of the following ways:
            A -> B -> idle -> A -> B -> idle -> A -> B.
            B -> A -> idle -> B -> A -> idle -> B -> A.

Input 2:
    A = "DBAD"
    B = 5
Output 2:
    7
    Explanation 2:
        D -> B -> A -> idle -> idle -> idle -> D
```

## Solution
### Editorial
```cpp
int Solution::solve(string A, int B) {
    unordered_map<char, int> record;
    int max_freq = 0;
    int max_tasks = 0;
    for(auto a : A) {
        record[a]++;
        max_freq = max(max_freq, record[a]);
    }
    for (auto task : record)
        if (task.second == max_freq)
            max_tasks++;
    int ans = (max_freq - 1) * (B+1) + max_tasks;
    return ans > A.size() ? ans : A.size();
}
```

### Fastest
```cpp
int Solution::solve(string A, int B) {
    vector<int> c(26, 0);
    for(char x : A)
        c[x-'A']++;
    sort(c.begin(), c.end(), [](int a, int b){ return a > b; });
    
    int gap = B * (c[0] - 1), ans = c[0] + gap;
    for (int i=1; i<26 && c[i] != 0; i++) {
        if (gap >= c[i]) {
            if (c[i] == c[0]) {
                gap -= (c[0] - 1);      
                ans++;
            } else {
                gap -= c[i];
            }
        } else {
            return A.length();
        }
    } 
    return ans;
}
```

### Lightweight
```cpp
typedef pair<int,int> pic;
struct cmp{
    bool operator()(const pic a, const pic b){
        return (a.second > b.second); /// small time
    }  
};

int Solution::solve(string A, int B){
    if(B==0) return A.size();
    vector<int> mp(26, 0);
    int max_freq = 0, max_task = 0;
    for(char c: A){
        mp[c-'A']++;
        max_freq = max(max_freq, mp[c-'A']);
    }
    
    //// D-3,B-2,C-2,A-2, and B =  2;
    //// so D_Btaks__D____D
    //// (max_freq-1)*B + max_task
    for(int i=0;i<26;i++){
        if(mp[i]>0 && mp[i]==max_freq){
            max_task++;
        }
    }
    int ans = (max_freq - 1)*(B+1) + max_task;
    return ans < A.size() ? A.size() : ans;
}
```


## References
* https://leetcode.com/problems/task-scheduler/
