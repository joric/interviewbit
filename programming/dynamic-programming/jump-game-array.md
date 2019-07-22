# Jump Game Array

https://www.interviewbit.com/problems/jump-game-array



## Hint 1

Greedy will work here. Think why.

Incase you are not a big fan of greedy approaches, there is a DP based solution which works here as well. 
Note that from an index i, you can choose to jump to any index in the range [i, i+A[i]]. Now if there is at least one index in the said range from where it is possible to jump to the end index, we are done. So if we start solving from end to start, and for every i, we loop j from i to i + A[i], and check if a solution is possible for j, then solution is possible for i.

This approach is however not linear. Take a moment and try to think if you can reduce this to O(n) approach.

## Hint 2

To move to linear approach, just maintain the minimum index which has solution possible till now. If its less than i+A[i]], then solution is possible for i and the minimum index gets updated to i.

## Solution

```cpp

// editorial

class Solution {
public:
    bool canJump(int A[], int n) {
        int minIndexPossible = n - 1;
        for (int i = n - 2; i >= 0; i--) {
            bool isPossibleFromThisIndex = false;
            if (i + A[i] >= minIndexPossible) {
                isPossibleFromThisIndex = true;
                minIndexPossible = i;
            }
            if (i == 0)
                return isPossibleFromThisIndex;
        }
        return true;
    }
};

// my solution

int Solution::canJump(vector<int> &jumps) {
    if (jumps.size() <= 1)
        return 1;
    vector<int> dp(jumps.size());
    int closest = jumps.size() - 1;
    for (int i = dp.size() - 2; i >= 0; i--) {
        if (jumps[i] - closest + i >= 0) {
            closest = i;
            dp[i] = 1;
        }
    }
    return dp[0];
}

// lightweight

int Solution::canJump(vector<int> &A) {
    if (A.size() == 0 || A.size() == 1) {
        return 1;
    }

    int j = A.size() - 1;
    int i = A.size() - 2;
    while (i >= 0) {
        if (i + A[i] < j) {
            if (i == 0) {
                return 0;
            }
        } else {
            j = i;
        }
        i--;
    }
    return 1;
}
```