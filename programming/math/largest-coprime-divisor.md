# Largest Coprime Divisor

https://www.interviewbit.com/problems/largest-coprime-divisor


## Solution

```cpp
int gcd(int A, int B) {
    if(A<B) swap(A,B);
    while(B) {
        int tmp = B;
        B = A%B;
        A = tmp;
    }
    return A;
}

int Solution::cpFact(int x, int y) {
    // first we will remove the common factors of x and y
    // from x by finding the greatest common divisor (gcd)
    // of x and y and dividing x with that gcd.
    // x = x / gcd(x, y)
    // we repeat till we get gcd(x, y) = 1.
    // At last, we return a = x
    while (gcd(x, y) != 1) {
        x = x / gcd(x, y);
    } 
    return x;
}
```