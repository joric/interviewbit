# Kth Permutation Sequence

https://www.interviewbit.com/problems/kth-permutation-sequence

This involves a little bit of maths and recursion for simplicity.

Number of permutation possible using n distinct numbers = n!

Lets first make k 0 based. 
Let us first look at what our first number should be. 
Number of sequences possible with 1 in first position: (n-1)!
Number of sequences possible with 2 in first position: (n-1)!
Number of sequences possible with 3 in first position: (n-1)!

Hence, the number at our first position should be k / (n-1)! + 1 th integer.

Can we reduce the k and modify the set we pick our numbers from
( initially 1 2 3 ... n ) to call the function for second position onwards ?

## Solution

```cpp

/* editorial */

class Solution {
  public:
    int factorial(int n) {
        if (n > 12) {
            // this overflows in int. So, its definitely greater than k
            // which is all we care about. So, we return INT_MAX which
            // is also greater than k.
            return INT_MAX;
        }
        // Can also store these values. But this is just < 12 iteration, so meh!
        int fact = 1;
        for (int i = 2; i <= n; i++)
            fact *= i;
        return fact;
    }

    string getPermutation(int k, vector<int> &candidateSet) {
        int n = candidateSet.size();
        if (n == 0) {
            return "";
        }
        if (k > factorial(n))
            return ""; // invalid. Should never reach here.

        int f = factorial(n - 1);
        int pos = k / f;
        k %= f;
        string ch = to_string(candidateSet[pos]);
        // now remove the character ch from candidateSet.
        candidateSet.erase(candidateSet.begin() + pos);
        return ch + getPermutation(k, candidateSet);
    }

    string getPermutation(int n, int k) {
        vector<int> candidateSet;
        for (int i = 1; i <= n; i++)
            candidateSet.push_back(i);
        return getPermutation(k - 1, candidateSet);
    }
};

/* my */

string Solution::getPermutation(int A, int B) {
    int i, arr[A];
    for (i = 0; i < A; i++)
        arr[i] = i + 1;
    for (i = 0; i < B - 1; i++)
        next_permutation(arr, arr + A);
    string str = "";
    for (i = 0; i < A; i++) {
        str += to_string(arr[i]);
    }
    return str;
}

/* my 2 */

string Solution::getPermutation(int A, int B) {
    vector<int> v(A);
    iota(v.begin(), v.end(), 1);

    for (int i = 1; i < B; i++)
        next_permutation(v.begin(), v.end());

    string s;
    for (int i = 0; i < A; i++)
        s += to_string(v[i]);

    return s;
}

/* my 3 */

string Solution::getPermutation(int A, int B) {
    vector<int> v(A);
    iota(v.begin(), v.end(), 1);
    while(--B) next_permutation(v.begin(), v.end());
    return accumulate(v.begin(), v.end(), string(), [](string&s, int i){return s+to_string(i);});
}

```

