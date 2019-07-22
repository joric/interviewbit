# Power Of Two Integers

https://www.interviewbit.com/problems/power-of-two-integers


Given a positive integer which fits in a 32 bit signed integer,
find if it can be expressed as A^P
where P > 1 and A > 0. A and P both should be integers.

Input: 4
Output: True  
as 2^2 = 4. 
## Solution

```cpp

int Solution::isPower (int A) {
    if (A <= 1)
        return true;

    int sqrtA = floor(sqrt (A));

    if (sqrtA*sqrtA==A)
        return 1;

    for (int x = 2; x <= sqrtA; x++) {
        unsigned int p = x;
        while (p <= A) {
            p *= x;
            if (p == A)
                return 1;
        }
    }

    return 0;
}

/*
int Solution::isPower (int A) {
    if (A <= 1)
        return 1;
    for (int i = 2; i<A; i++) {
        int x = i;
        for (int j=2; x<=A && j<32; j++) {
            x *= i;
            if (x == A)
                return 1;
        }
    }
    return 0;
}

*/
```