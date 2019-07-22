# Highest Product

https://www.interviewbit.com/problems/highest-product


## Solution

```cpp
#if 1 // O(n log n)

int Solution::maxp3(vector<int> &A) {
    auto n = A.size();
    sort(A.begin(), A.end());
    int allPositives = A[n-1] * A[n-2] * A[n-3];
    int twoNegatives = A[n-1] * A[0] * A[1];
    return max(allPositives, twoNegatives);
}

#else // O(n)

int Solution::maxp3(vector<int> &A) {
    int a, b, c, z, y;
    a = b = c = INT_MIN;
    z = y = INT_MAX;

    for (auto k : A) {
        if (k > a)
            c = b, b = a, a = k;
        else if (k > b)
            c = b, b = k;
        else if (k > c)
            c = k;

        if (k < z)
            y = z, z = k;
        else if (k < y)
            y = k;
    }
    //cout << a << " " << b << " " << c << " " << z << " " << y << " " << endl;

    return max(a * b * c, a * z * y);
}
#endif
```