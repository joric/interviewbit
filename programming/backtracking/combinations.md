# Combinations

https://www.interviewbit.com/problems/combinations



Combinations

Given two integers n and k, return all possible combinations of k numbers out of 1 2 3 ... n.

Make sure the combinations are sorted.

To elaborate,

Within every entry, elements should be sorted. [1, 4] is a valid entry while [4, 1] is not.
Entries should be sorted within themselves.
Example :
If n = 4 and k = 2, a solution is:

[
  [1,2],
  [1,3],
  [1,4],
  [2,3],
  [2,4],
  [3,4],
]

For every element, you have 2 options. You may either include the element in your subset or you will not include
the element in your subset. Make the call for both the cases.

## Solution

```cpp

/* editorial */

class Solution {
  public:
    void combineHelper(vector<int> &current, int n, int k, vector<vector<int>> &ans) {
        if (k == 0) {
            vector<int> newEntry = current;
            reverse(newEntry.begin(), newEntry.end());
            ans.push_back(newEntry);
            return;
        }
        if (n == 0 || n < k)
            return;
        // We have 2 options here. We can either include n or not.
        // Option 1 : Do not include n.
        combineHelper(current, n - 1, k, ans);
        // Option 2 : Include n.
        current.push_back(n);
        combineHelper(current, n - 1, k - 1, ans);
        current.pop_back();
        return;
    }

    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> current;
        combineHelper(current, n, k, ans);
        sort(ans.begin(), ans.end());
        return ans;
    }
};

/* my */

void make(vector<int> temp, int curr, int n, int k, vector<vector<int>> &ans) {
    if (temp.size() == k) {
        ans.push_back(temp);
        return;
    } else if (curr > n)
        return;

    for (int i = curr; i <= n; i++) {
        vector<int> t(temp);
        t.push_back(i);
        make(t, i + 1, n, k, ans);
    }
}

vector<vector<int>> Solution::combine(int n, int k) {
    vector<vector<int>> ans;
    for (int i = 1; i <= n; i++) {
        vector<int> temp(1, i);
        make(temp, i + 1, n, k, ans);
    }
    return ans;
}
```