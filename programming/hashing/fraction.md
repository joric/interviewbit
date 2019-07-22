# Fraction

https://www.interviewbit.com/problems/fraction


## Solution

```cpp
string Solution::fractionToDecimal(int numerator, int denominator) {

    int64_t n = numerator, d = denominator;
    if (n == 0) return "0";
    string res;
    if (n < 0 ^ d < 0) res += '-';
    n = abs(n), d = abs(d);
    res += to_string((n / d));
    if (n % d == 0) return res;
    res += '.';

    unordered_map<int, int> map;
    for (int64_t r = n % d; r; r %= d) {
        if (map.find(r) != map.end()) {
            res.insert(map[r], 1, '(');
            res += ')';
            break;
        }
        map[r] = res.size();
        r *= 10;
        res.push_back((char)('0' + (r / d)));
    }

    return res;
}
```