# Subsets II

https://www.interviewbit.com/problems/subsets-ii




Given a collection of integers that might contain duplicates, S, return all possible subsets.

 Note:
Elements in a subset must be in non-descending order.
The solution set must not contain duplicate subsets.
The subsets must be sorted lexicographically.
Example :
If S = [1,2,2], the solution is:

[
[],
[1],
[1,2],
[1,2,2],
[2],
[2, 2]
]

## Hint 1

Think in terms of recursion.

Make sure not to include repetitive elements in such a way that they get over-counted.

Solution approach

Think in terms of recursion.
This is very similar to the problem where you need to generate subsets for distinct integer.
However, in this case, because of repetitions, things are not as simple as choosing or rejecting an element. 
Now for the elements which are repeated you need to iterate over the count of elements you are going
to include in your subset.

## Solution

```cpp

// fastest

void helper(int index, vector<int> &A, vector<int> tempAns, vector<vector<int>> &ans) {
    for (int i = index; i < A.size(); i++) {
        tempAns.push_back(A[i]);
        ans.push_back(tempAns);
        helper(i + 1, A, tempAns, ans);
        while (i < A.size() - 1 && A[i] == A[i + 1])
            i++;
        tempAns.pop_back();
    }
}

vector<vector<int>> Solution::subsetsWithDup(vector<int> &A) {
    vector<vector<int>> ans;
    vector<int> tempAns;
    ans.push_back(tempAns);
    if (A.size() == 0)
        return ans;
    sort(A.begin(), A.end());
    helper(0, A, tempAns, ans);
    return ans;
}

// mine

void solve(vector<vector<int>> &res, vector<int> &row, vector<int> &v, int pos) {
    if (pos == v.size()) {
        res.push_back(row);
        return;
    }

    int index = pos + 1;
    while (index < v.size() && v[index] == v[pos])
        index++;

    for (int i = 0; i <= index - pos; i++) {

        for (int j = 0; j < i; j++)
            row.push_back(v[pos]);

        solve(res, row, v, index);

        for (int j = 0; j < i; ++j)
            row.pop_back();
    }
}

vector<vector<int>> Solution::subsetsWithDup(vector<int> &A) {
    vector<vector<int>> res;
    vector<int> row;
    sort(A.begin(), A.end());
    solve(res, row, A, 0);
    sort(res.begin(), res.end());
    return res;
}
```