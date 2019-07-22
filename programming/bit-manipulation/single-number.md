# Single Number

https://www.interviewbit.com/problems/single-number


	Given an array of integers, every element appears twice except for one. Find that single one.
## Solution

```cpp

/* editorial */
int Solution::singleNumber(const vector<int> &A) {
    int ret = 0;
    for(int i = 0; i < A.size(); i++)
        ret ^= A[i];
    return ret;
}


/* my solution */

int Solution::singleNumber(const vector<int> &A) {
    int ret = 0;
    for(int i = 0; i < A.size(); ret ^= A[i++]);
    return ret;
}

int Solution::singleNumber(const vector<int> &A) {
    int sum = A[0];
    for (int i=1; i<A.size(); i++) {
        sum ^= A[i];
    }
    return sum;
}
```