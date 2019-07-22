# Greatest Common Divisor

https://www.interviewbit.com/problems/greatest-common-divisor


## Solution

```cpp
int Solution::gcd(int A, int B) {
    if (B==0)
        return A;
    return gcd(B, A % B);
}

/*
int Solution::gcd(int A, int B) {
    if(A<B) swap(A,B);

    while(B) {
        int tmp = B;
        B = A%B;
        A = tmp;
    }

    return A;
}
*/
```