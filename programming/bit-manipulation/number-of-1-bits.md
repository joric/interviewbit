# Number Of 1 Bits

https://www.interviewbit.com/problems/number-of-1-bits


## Solution

```cpp
int Solution::numSetBits(unsigned int A) {
    // use kernigan
    int i;
    for (i = 0; A; i++) {
        A &= A - 1;
    }
    return i;
}

```