# 2 Sum

https://www.interviewbit.com/problems/2-sum



## Hint 1

O(n^2) runtime, O(1) space - Brute force:

The brute force approach is simple. Loop through each element x and find if there is another value that equals to target - x. As finding another value requires looping through the rest of array, its runtime complexity is O(n^2).

To improve on it, notice that when we fix one of the integers 'curValue', we know the value of the other integer we need to find ( target - curValue ). 
Then it becomes a simple search problem. You can store all the integers of the array in a hashmap and do a lookup to check if the elements exists in the map.

## Hint 2

Have you checked cases where the element you are looking up in the map is same as the curValue.

For example, consider the following cases :

A:[4 4] target: 8 
and A :[3 4] target: 8

The answer in first case should be [1 2] and in second case, it should be empty.


## Solution

```cpp

vector<int> Solution::twoSum(const vector<int> &A, int target) {
    unordered_map<int, int> m;
    for (int i = 0; i < A.size(); i++) {
        int comp = target - A[i];

        if (m.find(comp) != m.end())
            return { m[comp] + 1, i + 1 };

        if (m.find(A[i]) == m.end())
            m[A[i]] = i;
    }
    return {};
}

// fastest

vector<int> Solution::twoSum(const vector<int> &A, int B) {
    int n = A.size();
    vector<int> v;
    if (n < 2) {
        return v;
    }
    unordered_map<int, int> seen;
    seen.reserve(A.size());
    for (int i = 0; i < A.size(); ++i) {
        long long target = B - A[i];
        if (target < INT_MIN || target > INT_MAX) continue;
        auto it = seen.find(target);
        if (it != seen.end())
            return vector<int>{ it->second + 1, i + 1 };
        seen.insert(make_pair(A[i], i));
    }
    return vector<int>{};
}
```