# Add Binary Strings

https://www.interviewbit.com/problems/add-binary-strings


Given two binary strings, return their sum (also a binary string).

Example:

a = "100"

b = "11"
Return a + b = "111".


This is exactly like adding 2 numbers.

Note 1: It might be easier if you construct the reverse of the answer, reversing the strings that we have to add.
Note 2: Make sure you don't stop before carry is 0. (Cases like 11 + 1.)


Since sizes of two strings may be different, we first make the size of smaller string equal to that of bigger string by adding leading 0s for simplicity
Note that you can handle the addition without adding zeroes as well by adding a few if statements.
After making sizes same, one by one, we add bits from rightmost bit to leftmost bit.
In every iteration, we need to sum 3 bits: 2 bits of 2 given strings and carry.

The sum bit will be 1 if, either all of the 3 bits are set or one of them is set.
So we can add all the bits and then take remainder with 2 to get the current bit in the answer.

How to find carry?

Carry will be 1 if any of the two bits is set. So we can find carry by adding the bits and dividing the result by 2.

Following is a step by step algorithm:

Make them equal sized by adding 0s at the begining of smaller string.

Perform bit addition

Boolean expression for adding 3 bits a, b, c

Sum = (a + b + c) % 2
Carry = (a + b + c ) / 2

## Solution

```cpp


class Solution {
public:
    string addBinary(string a, string b) {
           string ans = "";
    int ansLen = 0, carry = 0, sum;
    int i = (int)a.length() - 1, j = (int)b.length() - 1;
    while (i >= 0 || j >= 0 || carry) {
        sum = carry;
        if (i >= 0) sum += (a[i] - '0');
        if (j >= 0) sum += (b[j] - '0');
        ans.push_back((char)('0' + sum % 2));
        carry = sum / 2;
        i--; 
        j--;
    }
    reverse(ans.begin(), ans.end());
    return ans;
    }
};


#if 0
string Solution::addBinary(string a, string b) {
    string result = ""; // Initialize result 
    int s = 0;          // Initialize digit sum 
  
    // Travers both strings starting from last 
    // characters 
    int i = a.size() - 1, j = b.size() - 1; 
    while (i >= 0 || j >= 0 || s == 1) 
    { 
        // Comput sum of last digits and carry 
        s += ((i >= 0)? a[i] - '0': 0); 
        s += ((j >= 0)? b[j] - '0': 0); 
  
        // If current digit sum is 1 or 3, add 1 to result 
        result = char(s % 2 + '0') + result; 
  
        // Compute carry 
        s /= 2; 
  
        // Move to next digits 
        i--; j--; 
    } 
    return result;     
}
#endif
```