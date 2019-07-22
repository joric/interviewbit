# Search for a Range Test

https://www.interviewbit.com/problems/search-for-a-range-test



A: [ 1 ]
B: 1
expected { 0, 0 }

A: [ 4, 7, 7, 7, 8, 10, 10 ]
B: 3
expected: -1 -1

## Solution

```cpp

#include <bits/stdc++.h>
using namespace std;

struct Solution{ vector<int> searchRange(const vector<int> &A, int B); };

#define DEBUG

vector<int> Solution::searchRange(const vector<int> &A, int B) {
    auto r = equal_range(A.begin(), A.end(), B);
    if (r.first!=A.end() && *r.first!=B)
        return {-1, -1};
    return { r.first-A.begin(), r.second-A.begin()-1 };
}

int main() {
    Solution s;
    //vector<int> res = s.searchRange({ 4, 7, 7, 7, 8, 10, 10 },3);
    vector<int> res = s.searchRange({1},1);
    cout << res[0] << "," << res[1] << endl;
    
}
```