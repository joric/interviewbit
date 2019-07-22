# Permutations

https://www.interviewbit.com/problems/permutations


## Solution

```cpp
void helper(vector<vector<int>> &res, vector<int> &set, int pos) {
    if (pos == set.size()) {
        res.push_back(set);
        return;
    }
    for (int i = pos; i < set.size(); i++) {
        swap(set[i], set[pos]);
        helper(res, set, pos + 1);
        swap(set[i], set[pos]);
    }
}

vector<vector<int>> Solution::permute(vector<int> &A) {
    vector<vector<int>> res;
    helper(res, A, 0);
    return res;
}
```