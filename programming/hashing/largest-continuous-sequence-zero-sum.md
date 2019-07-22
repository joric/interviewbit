# Largest Continuous Sequence Zero Sum

https://www.interviewbit.com/problems/largest-continuous-sequence-zero-sum



## Hint 1

Lets try to reduce the problem to a much simpler problem. 
Lets say we have another array sum where

  sum[i] = Sum of all elements from A[0] to A[i]
Note that in this array, sum of elements from A[i] to A[j] = Sum[j] - Sum[i-1]

We need to find j and i such that sum of elements from A[i] to A[j] = 0
 Or Sum[j] - Sum[i-1] = 0
 Or Sum[j] = Sum[i-1]
Now, the problem reduces to finding 2 indices i and j such that sum[i] = sum[j] 
How can you solve the above problem ?

## Hint 2


There are two steps:
1. Create cumulative sum array where ith index in this array represents total sum from 1 to ith index element.
2. Iterate all elements of cumulative sum array and use hashing to find two elements where value at ith index == value at jth index but i != j.
3. IF element is not present in hash in fill hash table with current element.

## Solution

```cpp

vector<int> Solution::lszero(vector<int> &A) {
    int x = 0, y = -1;
    unordered_map<int, int> m;

    int sum = 0, max_dist = -1, dist = 0;

    m[0] = -1;

    for (int i = 0; i < A.size(); i++) {
        sum += A[i];
        if (m.count(sum) == 0) {
            m[sum] = i;
        } else {
            int tmp = m[sum] + 1;
            dist = i - tmp + 1;
            if (dist > max_dist) {
                max_dist = dist;
                x = tmp;
                y = i;
            }
        }
    }

    return vector<int>(A.begin() + x, A.begin() + y + 1);
}
//////////////////////

vector<int> Solution::lszero(vector<int> &A) {
    unordered_map<int, int> m;
    vector res;
    int n = A.size();
    int sum = 0;
    int length = INT_MIN;
    int a = 0;
    int b = -1;

    for (int i = 0; i <= n - 1; i++) {
        sum += A[i];

        if (m.find(sum) == m.end()) {
            m[sum] = i;
        } else {
            if ((i - (m[sum] + 1)) > b - a) {
                b = i;
                a = m[sum] + 1;
            }
        }

        if (sum == 0) {
            if (i > b - a) {
                b = i;
                a = 0;
            }
        }
    }

    return vector<int>(A.begin() + x, A.begin() + y + 1);
}

///// editorial

vector<int> Solution::lszero(vector<int> &A) {
    unordered_map<int, int> mp;
    int max_length = -1;
    int start = 0, end = 0;
    int sum = 0;

    for (int i = 0; i < A.size(); i++) {
        sum += A[i];

        if (A[i] == 0 && max_length == -1) {
            max_length = 1;
            start = i;
            end = i;
        }

        if (sum == 0 && i + 1 > max_length) {
            max_length = i + 1;
            start = 0;
            end = i;
        }

        if (mp.find(sum) != mp.end()) {
            int x = i - mp[sum];
            if (x > max_length) {
                max_length = x;
                start = mp[sum] + 1;
                end = i;
            }
        }

        else {
            mp.insert(make_pair(sum, i));
        }
    }

    vector<int> ans;
    if (max_length == -1)
        return ans;

    for (int i = start; i <= end; i++) {
        ans.push_back(A[i]);
    }
    return ans;
}
```