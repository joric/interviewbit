# Atoi

https://www.interviewbit.com/problems/atoi



## Hint 1

Think of all possible cases to handle as tricky cases.

You also need to take special care for numbers which overflows. How?

## Hint 2

Access Hint
We only need to handle four cases:

Discards all leading whitespaces

Sign of the number

Overflow

Invalid input

Detecting overflow gets tricky. You need to detect overflow before the number is about to overflow.

So:

If the number is positive and the current number is greater than MAX_INT/10, and you are about to append a digit, then certainly your number will overflow.

If the current number = MAX_INT / 10, then based on the last digit, we can detect if the number will overflow.

## Solution

```cpp

/* editorial */

class Solution {
  public:
    int atoi(const string &str) {
        int sign = 1, base = 0, i = 0;
        while (str[i] == ' ') {
            i++;
        }
        if (str[i] == '-' || str[i] == '+') {
            sign = (str[i++] == '-') ? -1 : 1;
        }
        while (str[i] >= '0' && str[i] <= '9') {
            if (base > INT_MAX / 10 || (base == INT_MAX / 10 && str[i] - '0' > 7)) {
                if (sign == 1)
                    return INT_MAX;
                else
                    return INT_MIN;
            }
            base = 10 * base + (str[i++] - '0');
        }
        return base * sign;
    }
};

#include <bits/stdc++.h>
using namespace std;

struct Solution {
    int atoi(const string A);
};

int Solution::atoi(const string A) {
    long long res = 0;
    const char *p = A.c_str();

    while (*p && *p == ' ')
        p++;

    int sign = 1;
    if (*p == '-' || *p == '+') {
        sign = *p == '-' ? -1 : 1;
        p++;
    }

    while (*p) {
        if (!isdigit(*p))
            break;
        int digit = *p - '0';
        res = res * 10 + digit;
        if (res > INT_MAX || res < INT_MIN)
            return sign < 0 ? INT_MIN : INT_MAX;
        p++;
    }
    return res * sign;
}

int main() {
    Solution c;
    cout << c.atoi("5121478262 8070067M75 X199R 547 8C0A11 93I630 4P4071 029W433619 M3 5 14703818 776366059B9O43393") << endl;
}

/* another solution */
int Solution::atoi(const string A) {
    int sign = 1, res = 0;
    const char *p = A.c_str();

    if (*p=='-' || *p=='+') {
        sign = *p=='-' ? -1 : 1;
        p++;
    }

    while (*p >= '0' && *p <= '9') {
        if (res >= INT_MAX / 10)
            return sign == -1 ? INT_MIN : INT_MAX;
        res = res * 10 + (*p++ -'0');
    }
    
    return res * sign;
}

```