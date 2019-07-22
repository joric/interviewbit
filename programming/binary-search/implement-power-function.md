# Implement Power Function

https://www.interviewbit.com/problems/implement-power-function


Implement pow(x, n) % d.
In other words, given x, n and d,
find (xn % d)
Note that remainders on division cannot be negative. 
In other words, make sure the answer you return is non negative.
## Solution

```cpp

/* editorial */

int pow (int x, int n, int p) {
    if (n == 0)
        return 1 % p;
    long long ans = 1, base = x;

    while (n > 0) {
        if (n % 2 == 1) {
            ans = (ans * base) % p;
            n--;
        } else {
            base = (base * base) % p;
            n /= 2;
        }
    }

    if (ans < 0)
        ans = (ans + p) % p;
    return ans;
}


int Solution::pow (int x, int n, int d) {
    if (n == 1) {
        if (x < 0)
            return (x % d) + d;
        return x;
    }
    if (n == 0 && x == 0)
        return 0;
    if (n == 0)
        return 1 % d;
    long long temp = pow (x, n / 2, d) % d;
    long long mul = (temp * temp) % d;
    if (mul < 0)
        mul = mul + d;
    if (n % 2 == 0)
        return mul;
    return (mul * x) % d;
}


int Solution::pow (int x, int n, int d) {
    if (n == 0)
        return 1 % d;

    long long ans = 1, base = x;
    while (n > 0) {
        if (n % 2 == 1) {
            ans = (ans * base) % d;
            n--;
        }
        else {
            base = (base * base) % d;
            n /= 2;
        }
    }
    if (ans < 0)
        ans = (ans + d) % d;
    return ans;
}

int Solution::pow (int x, int n, int d) {
    map < pair < int, int >, long long int >solutionStore;

    pair < int, int >curr = make_pair (x, n);

    if (solutionStore.find (curr) != solutionStore.end ()) {
        return solutionStore.find (curr)->second;
    }

    if (x == 0)
        return 0;

    if (n == 0)
        return 1;

    if (n == 1)
        return max (x % d, (x % d) + d);

    long long int ans = 1;

    ans *= pow (x, n / 2, d);
    ans = ans % d;

    ans *= pow (x, n - n / 2, d);
    ans = ans % d;

    if (ans < 0)
        ans = ans + d;

    solutionStore[curr] = ans;
    return ans;
}
```