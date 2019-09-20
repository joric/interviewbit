# 0/1 String

https://www.interviewbit.com/problems/0-1-string/

A "0/1 string" is a string in which every character is either 0 or 1.
There are two operations that can be performed on a 0/1 string:

* switch: Every 0 becomes 1 and every 1 becomes 0. For example, "100" becomes "011".
* reverse: The string is reversed. For example, "100" becomes "001".

Consider this infinite sequence of 0/1 strings:

```
S0 = ""
S1 = "0"
S2 = "001"
S3 = "0010011"
S4 = "001001100011011"
...
S(n) = S(n-1) + "0" + switch(reverse(S(n-1)))
```

Given an integer A,
find and return the Ath character of Sx, where x = 10^100.

### Input Format

The only argument given is integer A.

### Output Format

Return the total number of digit **1** appearing in all non-negative integers less than or equal to A.

### Constraints

1 <= A <= 10^9

### For Example

```
Input 1:
    A = 10
Output 1:
    0

Input 2:
    A = 3
Output 2:
    1
```

## Solution

### Mine (python)
```python
class Solution:
    def solve(self, k):
        return 0 if k & ((k & -k) << 1) == 0 else 1
````
