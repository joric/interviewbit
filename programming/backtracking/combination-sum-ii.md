# Combination Sum II

https://www.interviewbit.com/problems/combination-sum-ii


Given a collection of candidate numbers (C) and a target number (T),
find all unique combinations in C where the candidate numbers sums to T.

Each number in C may only be used once in the combination.

1. Elements in a combination (a[1], a[2], ... , a[k]) must be in non-descending order.
(ie, a[1] <= a[2] <= ... a[k]).
2. The solution set must not contain duplicate combinations.

### Example

Given candidate set 10,1,2,7,6,1,5 and target 8,

A solution set is:

```
[1, 7]
[1, 2, 5]
[2, 6]
[1, 1, 6]
```

### Warning

DO NOT USE LIBRARY FUNCTION FOR GENERATING COMBINATIONS.

Example: itertools.combinations in python.

If you do, we will disqualify your submission retroactively and give you penalty points. 


## Hint 1

Think how can you use recursion with current index and target sum in order to generate all combinations.

Also, you will have to take special care of those elements which can be overcounted as they are repeated.

Some elements can be repeated in the input set. Make sure you iterate over the number of occurrences
of those elements to make sure you are not counting the same combinations again.

Once you do that, things are fairly straightforward. You make a recursive call with the remaining
sum and make sure the indices are moving forward.

## Solution

### Editorial

```cpp
void solve(vector<int> &A, int start, int end, vector<int> &temp, set<vector<int>> &ans, int sum, unordered_map<int, int> &used) {
    if (sum < 0 || start > end) return;
    if (sum == 0) {
        ans.insert(temp);
        return;
    }

    solve(A, start + 1, end, temp, ans, sum, used);
    if (used[A[start]] > 0) {
        temp.push_back(A[start]);
        used[A[start]]--;
        solve(A, start, end, temp, ans, sum - A[start], used);
        temp.pop_back();
        used[A[start]]++;
    }
}

vector<vector<int>> Solution::combinationSum(vector<int> &A, int B) {
    sort(A.begin(), A.end());
    unordered_map<int, int> used;
    for (int a : A)
        used[a]++;

    vector<int> temp;
    set<vector<int>> ans;
    vector<vector<int>> ret;
    int n = A.size();

    solve(A, 0, n - 1, temp, ans, B, used);
    for (auto it = ans.begin(); it != ans.end(); it++)
        ret.push_back(*it);

    return ret;
}
```
### Mine
```cpp
void backtracking(int start, vector<int> &row, int sum, vector<vector<int>> &res, vector<int> &v, int target) {
    if (sum == target) {
        res.push_back(row);
        return;
    }

    if (sum > target || start == v.size())
        return;

    row.push_back(v[start]);
    sum += v[start];
    backtracking(start + 1, row, sum, res, v, target);
    sum -= row[row.size() - 1];
    row.pop_back();

    int endIndex = 0;
    for (endIndex = start + 1; endIndex < v.size() && v[endIndex] == v[start]; ++endIndex)
        ++start;

    backtracking(start + 1, row, sum, res, v, target);
}

vector<vector<int>> Solution::combinationSum(vector<int> &v, int target) {
    vector<vector<int>> res;
    vector<int> row;
    sort(v.begin(), v.end());
    backtracking(0, row, 0, res, v, target);
    return res;
}
```
