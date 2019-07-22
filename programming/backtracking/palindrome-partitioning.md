# Palindrome_partitioning

https://www.interviewbit.com/problems/palindrome_partitioning

Given a string s, partition s such that every string of the partition is a palindrome.

Return all possible palindrome partitioning of s.

For example, given s = "aab",
Return

```
[
  ["a","a","b"]
  ["aa","b"],
]
```

Ordering the results in the answer : Entry i will come before Entry j if :
```
1. len(Entryi[0]) < len(Entryj[0]) OR
2. (len(Entryi[0]) == len(Entryj[0]) AND len(Entryi[1]) < len(Entryj[1])) OR
*
*
*
3. (len(Entryi[0]) == len(Entryj[0]) AND ... len(Entryi[k] < len(Entryj[k]))
```

In the given example,

["a", "a", "b"] comes before ["aa", "b"] because len("a") < len("aa")



## Solution

```cpp
inline bool isPalindrome(string A) {
    string rev = A;
    reverse(rev.begin(), rev.end());
    return (rev == A);
}

void GeneratePartitions(vector<string> set, string A, vector<vector<string>> &result) {
    if (A.empty()) {
        result.push_back(set);
        return;
    }

    for (int l = 1; l <= A.length(); ++l) {
        string first_part = A.substr(0, l);
        string second_part = A.substr(l);

        if (isPalindrome(first_part)) {
            vector<string> s = set;
            s.push_back(first_part);
            GeneratePartitions(s, second_part, result);
        }
    }
}

vector<vector<string>> Solution::partition(string A) {
    vector<vector<string>> result;
    vector<string> empty_set;
    GeneratePartitions(empty_set, A, result);
    sort(result.begin(), result.end());
    return result;
}

/* my */

bool isPalindrome(string &s, int i, int j) {
    while (i < j)
        if (s[i++] != s[j--])
            return false;
    return true;
}

void backtracking(string &s, int i, vector<string> &row, vector<vector<string>> &res) {
    if (i == s.length()) {
        res.push_back(row);
        return;
    }

    for (int x = i; x < s.length(); ++x) {
        if (isPalindrome(s, i, x)) {
            row.push_back(s.substr(i, x - i + 1));
            backtracking(s, x + 1, row, res);
            row.pop_back();
        }
    }
}

vector<vector<string>> Solution::partition(string A) {
    vector<string> row;
    vector<vector<string>> res;
    backtracking(A, 0, row, res);
    return res;
}
```