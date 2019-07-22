# All Factors

https://www.interviewbit.com/problems/all-factors


## Solution

```cpp
vector<int> Solution::allFactors(int A) {
    vector<int> factors;
    int sqrtA = int(sqrt(A));
    for (int i=1; i<=sqrtA; i++) {
        if (A % i==0) {
            factors.push_back(i);
            if (i!=sqrtA) {
                factors.push_back(A/i);
            }
        }
    }
    sort(factors.begin(), factors.end());
    return factors;
}
```