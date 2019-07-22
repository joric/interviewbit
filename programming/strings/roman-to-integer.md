# Roman To Integer

https://www.interviewbit.com/problems/roman-to-integer

 editorial */

class Solution {
  public:
    int romanToInt(string s) {
        int num = 0;
        int size = s.size();

        for (int i = 0; i < size; i++) {
            // Does lesser value precede higher value ?
            if (i < (size - 1) && romanCharToInt(s[i]) < romanCharToInt(s[i + 1])) {
                num -= romanCharToInt(s[i]);
            } else {
                num += romanCharToInt(s[i]);
            }
        }
        return num;
    }

    int romanCharToInt(char c) {
        switch (c) {
        case 'I': return 1;
        case 'V': return 5;
        case 'X': return 10;
        case 'L': return 50;
        case 'C': return 100;
        case 'D': return 500;
        case 'M': return 1000;
        default: return 0;
        }
    }
};

/* my solution */

int Solution::romanToInt(string s) {
    unordered_map<char, int> m = { { 'I', 1 }, { 'V', 5 }, { 'X', 10 }, { 'L', 50 }, { 'C', 100 }, { 'D', 500 }, { 'M', 1000 } };
    int x = 0;
    for (int i = 0; i < s.length(); i++)
        x += i + 1 < s.length() && m[s[i]] < m[s[i + 1]] ? m[s[i + 1]] - m[s[i++]]: m[s[i]];
    return x;
}

/* another solution */

int Solution::romanToInt(string s) {
    unordered_map<char, int> m = { { 'I', 1 }, { 'V', 5 }, { 'X', 10 }, { 'L', 50 }, { 'C', 100 }, { 'D', 500 }, { 'M', 1000 } };
    int x = 0, n = s.size();
    for (int i = 0; i < n; i++)
        x += i < n-1 && m[s[i]] < m[s[i+1]] ? - m[s[i]]: m[s[i]];
    return x;
}
```