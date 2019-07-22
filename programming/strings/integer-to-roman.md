# Integer To Roman

https://www.interviewbit.com/problems/integer-to-roman


Given an integer, convert it to a roman numeral, and return a string corresponding to its roman numeral version

Input is guaranteed to be within the range from 1 to 3999.

Example :

Input: 5
Return: "V"

Input: 14
Return: "XIV"
 Note: This question has a lot of scope of clarification from the interviewer. Please take a moment to think of all the needed clarifications and see the expected response using "See Expected Output" For the purpose of this question, https://projecteuler.net/about=roman_numerals has very detailed explanations. 

## Solution

```cpp


/* editorial */

class Solution {
  public:
    string intToRoman(int num) {
        string M[] = { "", "M", "MM", "MMM" };
        string C[] = { "", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM" };
        string X[] = { "", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC" };
        string I[] = { "", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX" };
        return M[num / 1000] + C[(num % 1000) / 100] + X[(num % 100) / 10] + I[num % 10];
    }
};

/* my solution */

string intToRoman(int num) {
    string value[] = { "M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I" };
    int key[] = { 1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1 };
    string ret;
    int iter = 0;
    while (num > 0) {
        while (num >= key[iter]) {
            ret += value[iter];
            num -= key[iter];
        }
        iter++;
    }
    return ret;
}
```