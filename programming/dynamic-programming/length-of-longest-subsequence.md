# Length Of Longest Subsequence

https://www.interviewbit.com/problems/length-of-longest-subsequence




Given an array of integers, find the length of longest subsequence which is first increasing then decreasing.

**Example: **

For the given array [1 11 2 10 4 5 2 1]

Longest subsequence is [1 2 10 4 2 1]

Return value 6

## Hint 1

Try to come up with something which stores length of increasing sequence upto a particular index and length of decreasing sequence from that particular index.

Can you think of DP for either of the task.

Solution approach

Construct array inc[i] where inc[i] stores Longest Increasing subsequence ending with A[i].
This can be done simply with O(n^2) DP.

Construct array dec[i] where dec[i] stores Longest Decreasing subsequence ending with A[i].
This can be done simply with O(n^2) DP.

Now we need to find the maximum value of (inc[i] + dec[i] - 1)

## Solution

### Editorial

```cpp
int Solution::longestSubsequenceLength(const vector &A) {
    int n = A.size();
    int inc[n];
    int dec[n];
    int ct = 0;

    inc[0] = 1;
    for (int i = 1; i < n; i++) {
        inc[i] = 1;
        for (int j = i - 1; j >= 0; j--) {
            if (A[i] > A[j] && inc[i] < inc[j] + 1)
                inc[i] = inc[j] + 1;
        }
    }

    dec[n - 1] = 1;
    for (int i = n - 2; i >= 0; i--) {
        dec[i] = 1;
        for (int j = i + 1; j < n; j++) {
            if (A[i] > A[j] && dec[i] < dec[j] + 1)
                dec[i] = dec[j] + 1;
        }
    }

    int mx = 0;
    for (int i = 0; i < n; i++)
        mx = max(mx, inc[i] + dec[i] - 1);

    return mx;
}
```
### My

```cpp
int fn(const vector<int> &A, int curr, int last, int len, bool inc) {
    int n = A.size();
    if (curr == n) {
        return len;
    }

    int ans = len;

    for (int i = curr; i < n; i++) {
        if (inc) {
            if (A[i] > last) {
                ans = max(ans, fn(A, i + 1, A[i], len + 1, inc));
            } else if (A[i] < last) {
                ans = max(ans, fn(A, i + 1, A[i], len + 1, false));
            }
        } else {
            if (A[i] < last) {
                ans = max(ans, fn(A, i + 1, A[i], len + 1, inc));
            }
        }
    }

    return ans;
}

int Solution::longestSubsequenceLength(const vector<int> &A) {
    if (A.size() == 0) {
        return 0;
    }

    // Recursive solution
    // return fn(A, 1, A[0], 1, true);

    // DP solution
    int n = A.size(), sol = 1;

    vector<int> inc(n, 1);
    vector<int> dec(n, 1);
    vector<int> ans(n, 1);

    for (int i = 1; i < n; i++) {
        for (int j = i - 1; j >= 0; j--) {
            if (A[i] > A[j]) {
                inc[i] = max(inc[i], inc[j] + 1);
                ans[i] = max(ans[i], inc[i]);
            } else if (A[i] < A[j]) {
                dec[i] = max(dec[i], dec[j] + 1);
                ans[i] = max(ans[i], max(dec[i], ans[j] + 1));
            }
        }
        sol = max(sol, ans[i]);
    }

    return sol;
}
```