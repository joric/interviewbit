# Subset

https://www.interviewbit.com/problems/subset


## Solution

```cpp
void helper(vector<vector<int>> &res, vector<int> &A, vector<int> &subset, int index) {
    res.push_back(subset);
    for (int i = index; i < A.size(); i++) {
        subset.push_back(A[i]);
        helper(res, A, subset, i+1);
        subset.pop_back();
    }
}

vector<vector<int> > Solution::subsets(vector<int> &A) {
    sort(A.begin(), A.end());
    vector<vector<int>> res;
    vector<int> subset;
    helper(res, A, subset, 0);
    return res;
}

```