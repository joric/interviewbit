# 4 Sum

https://www.interviewbit.com/problems/4-sum



## Hint 1

The naive approach obviously is exploring all combinations of 4 integers using 4 loops.
Now, to look into improving, does it help if we sort the array ?
Also, think of how your approach would change if you did not have
to list all of the unique tuples but just tell whether at least one such tuple existed ( YES / NO ).

## Hint 2

When the array is sorted, try to lock the least and second least integer by looping over it.
Lets say the least integer in the solution is arr[i] and second least is arr[j].
Now we need to find a pair of integers k and l such that arr[k] + arr[l] is target-arr[i]-arr[j]. 
To do that, lets try the 2 pointer approach. If we lock the two pointers at the end ( that is, j+1 and end of array ),
we look at the sum.
If the sum is smaller than the sum we want, we increase the first pointer to increase the sum.
If the sum is bigger than the sum we want, we decrease the end pointer to reduce the sum.

Note that there is one more solution possible if the question only asked to answer
YES / NO to suggest whether there existed at least one tuple with the target sum. 
Then we could have gone with an approach using more memory with hashing.


## Solution

```cpp


// editorial

#define F first
#define S second

vector<vector<int> > Solution::fourSum(vector<int> &A, int B) {
    int n=A.size();
    sort(A.begin(), A.end());
    unordered_map<int, vector<pair<int,int>>> m;
    for (int i=0; i<n; i++){
        for (int j=i+1; j<n; j++){
            m[A[i]+A[j]].push_back({i,j});
        }
    }
    set<vector<int>> s;
    vector<vector<int>> v;
    for (int i=0; i<n; i++){
        for (int j=i+1; j<n; j++){
            for (auto it: m[B-A[i]-A[j]]){
                if (i!=it.F && i!=it.S && j!=it.F && j!=it.S){
                     vector <int> v = {A[i], A[j], A[it.F], A[it.S]};
                     sort(v.begin(), v.end());
                     s.insert(v);
                }
            }
        }
    }
    for (auto it : s)
        v.push_back(it);
    return v;
    
}

/// mine

vector<vector<int>> Solution::fourSum(vector<int> &A, int target) {
    sort(A.begin(), A.end());
    vector<vector<int>> res;
    auto size = A.size();
    for (auto i = 0; i < size - 3; ++i) {
        if (i > 0 && A[i] == A[i - 1])
            continue;
        for (auto j = i + 1; j < size - 2; ++j) {
            if (j > i + 1 && A[j] == A[j - 1])
                continue;

            int p = j + 1, q = size - 1;
            while (p < q) {
                auto sum = A[i] + A[j] + A[p] + A[q];
                if (sum == target) {
                    res.push_back({A[i], A[j], A[p], A[q]});
                    p++;
                    while (p < q && A[p] == A[p - 1])
                        p++;
                } else if (sum > target)
                    q--;
                else if (sum < target)
                    p++;
            }
        }
    }
    return res;
}
```