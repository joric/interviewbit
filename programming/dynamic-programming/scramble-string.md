# Scramble String

https://www.interviewbit.com/problems/scramble-string/

Given a string s1, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.

Below is one possible representation of s1 = "great":

```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```
To scramble the string, we may choose any non-leaf node and swap its two children.

For example, if we choose the node "gr" and swap its two children, it produces a scrambled string "rgeat".
```
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```
We say that "rgeat" is a scrambled string of "great".

Similarly, if we continue to swap the children of nodes "eat" and "at", it produces a scrambled string "rgtae".
```
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
```
We say that "rgtae" is a scrambled string of "great".

Given two strings s1 and s2 of the same length, determine if s2 is a scrambled string of s1.
Return 0/1 for this problem.

## Hint 1
Hint : Divide and Conquer + Dynamic Programming

On every step, you need to figure out if there exists one of the positions at root, using which the 2 parts of strings are scrambled strings of each other.

Try writing a bruteforce solution and see which steps can be memoized.

## Solution Approach


Lets first think of a bruteforce solution. 
Obviously the 2 strings need to have the same number of characters and the same character set, otherwise the answer is definitely no.

In the bruteforce solution, we loop to find out the root of the tree. 
Lets say the root is the ith character of string s1. Then the first part of s1 [0…i) can either match ( be a scrambled string of ) to s2[0…i) or s2(i+1 .. L]. Depending on which part s1[0…i) matches to, we match the remaining part of s1 to remaining part of s2. Just to clarify when we say s1 matches s2, we mean s1 is a scrambled string of s2.

We can write a very easy recursive function for this.

```
public boolean isScramble(String s1, String s2) {
    // CHECK BASE CASES HERE

    if (s1 not anagram of s2) return false;
    
    for(int i = 1; i < s1.length(); i++) { // i being the root position
        if(isScramble(s1.substring(0,i), s2.substring(0,i)) && isScramble(s1.substring(i), s2.substring(i))) return true;
        if(isScramble(s1.substring(0,i), s2.substring(s2.length()-i)) && isScramble(s1.substring(i), s2.substring(0, s2.length()-i))) return true;
    }
    return false;
}
```

Now note that in any call for this function, the strings s1 and s2 would always be substrings of original S1 and S2. And we can specify substrings using startIndex, endIndex.

Which means the recursive function can only be called with (startIndex1, endIndex1, startIndex2, endIndex2) possible tuples which can only take LLL*L possibilities roughly.

Memoization ?

Additional Hint :

Function calls are only valid if endIndex1 - startIndex1 = endIndex2 - startIndex2. 
Can you just look at (startIndex1, endIndex1, startIndex2) and infer the endIndex2 based on these values ? 
Or keep passing around (startIndex1, startIndex2, length) ?


## Solution

### Editorial
```cpp
int Solution::isScramble(const string s1, const string s2) {
    int len = s1.size();
    bool dp[len + 1][len + 1][len + 1];
    memset(dp, 0, sizeof(dp));

    for (int i = len - 1; i >= 0; i--) {
        for (int j = len - 1; j >= 0; j--) {
            dp[i][j][1] = (s1[i] == s2[j]);
            for (int l = 2; i + l <= len && j + l <= len; l++) {
                for (int n = 1; n < l; n++) {
                    dp[i][j][l] |= dp[i][j][n] && dp[i + n][j + n][l - n];
                    dp[i][j][l] |= dp[i][j + l - n][n] && dp[i + n][j][l - n];
                }
            }
        }
    }
    return dp[0][0][len];
}
```

### Mine
```cpp
int Solution::isScramble(string A, string B) {

    int length1 = A.length();
    //int length2 = B.length();

    if (length1 != B.length())
        return false;
    if (A == B)
        return true;

    bool scrambled[length1][length1][length1];
    for (int i = 0; i < length1; ++i) {
        for (int j = 0; j < length1; ++j) {
            scrambled[i][j][0] = (A[i] == B[j]);
        }
    }

    for (int k = 1; k < length1; ++k) {
        for (int i = 0; i < length1 - k; ++i) {
            for (int j = 0; j < length1 - k; ++j) {
                scrambled[i][j][k] = false;
                for (int p = 0; p < k; ++p) {
                    if ((scrambled[i][j][p] && scrambled[i + p + 1][j + p + 1][k - p - 1]) || (scrambled[i][j + k - p][p] && scrambled[i + p + 1][j][k - p - 1])) {
                        scrambled[i][j][k] = true;
                        break;
                    }
                }
            }
        }
    }

    return scrambled[0][0][length1 - 1];
}
```
