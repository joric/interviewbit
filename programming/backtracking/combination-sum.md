# Combination Sum

https://www.interviewbit.com/problems/combination-sum


Given a set of candidate numbers (C) and a target number (T), find all unique combinations
in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

 

Think how can you use recursion with current index and target sum in order to generate all combinations.


In every recursion run, you either include the element in the combination or you don't.
To account for multiple occurrences of an element, make sure you call
the next function without incrementing the current index.

## Solution

```cpp

// fastest

void solve(vector<int> &A,int start,int end,vector<int>& temp,set<vector<int> >& ans,int sum)
{
    if(sum<0) return;
    if(sum==0){
        ans.insert(temp);
        return;
    }
    if(start>end) return;
    solve(A,start+1,end,temp,ans,sum);
    temp.push_back(A[start]);
    solve(A,start,end,temp,ans,sum-A[start]);
    temp.pop_back();
}

vector<vector<int> > Solution::combinationSum(vector<int> &A, int B) {
    sort(A.begin(), A.end());
    int n=A.size(),i;
    vector<vector<int> > ret;
    if(n==0) return ret;
    set<vector<int> > ans;
    vector<int> temp;
    solve(A,0,n-1,temp,ans,B);
    for(auto it=ans.begin() ; it!=ans.end() ; it++)
        ret.push_back(*it);
    return ret;
}

// mine

void backtracking(int start, vector<int> &row, int sum, vector<vector<int>> &res, vector<int> &v, int target) {
    if (sum >= target) {
        if (sum == target)
            res.emplace_back(row);
        return;
    }
    if (start == v.size())
        return;
    row.push_back(v[start]);
    sum += v[start];
    backtracking(start, row, sum, res, v, target);
    sum -= row[row.size() - 1];
    row.pop_back();
    backtracking(start + 1, row, sum, res, v, target);
}

vector<vector<int>> Solution::combinationSum(vector<int> &v, int target) {
    vector<vector<int>> res;
    vector<int> row, a;
    sort(v.begin(), v.end());

    a.push_back(v[0]);
    for (auto i = 1; i < v.size(); ++i)
        if (v[i - 1] != v[i])
            a.push_back(v[i]);

    backtracking(0, row, 0, res, a, target);
    return res;
}
```