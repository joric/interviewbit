# Diffk II

https://www.interviewbit.com/problems/diffk-ii


## Hint 1

The naive approach obviously is exloring all combinations of 2 integers using 2 loops and then check their difference.

However, lets look at it like this. 
We are looking to find pair of integers where A[i] - A[j] = k, k being known entity
Lets say we lock A[i] ( i.e. we know A[i]), do we know what A[j] should be ?
Once we know what A[j] we want, does it reduce to a search / lookup problem ?

Solution approach

We are looking to find pair of integers where A[i] - A[j] = k, k being known entity
Lets say we lock A[i] ( i.e. we know A[i]), do we know what A[j] should be ?
A[j] = A[i] - k.

We can store all the numbers in a hashmap / hashset and then lookup A[j] in it to find out if A[j] exists.

Corner case: How do you handle case when k = 0 cleanly ?


## Solution

```cpp


// fastest

int Solution::diffPossible(const vector<int> &A, int k) {
    unordered_set<int> s;
    for (auto x:A) {
        if(s.count(x-k) || s.count(x+k))
            return true;
        s.insert(x);
    }
    return false;
}

// misc

int Solution::diffPossible(const vector<int> &A, int B) {
    vector<int> input(A);
    sort(input.begin(), input.end());
    auto p = 0, q = 0;
    auto size = input.size();
    while (q < size)
    {
        if (p == q)
            ++q;
        else if (input[q]-input[p] == B && p!=q)
            return 1;
        else if (input[q]-input[p] < B)
            ++q;
        else if (input[q]-input[p] > B)
            ++p;
    }
    return 0;
}
```