# Regular Expression II

https://www.interviewbit.com/problems/regular-expression-ii/

Implement regular expression matching with support for '.' and '*'.

1. '.' Matches any single character.
2. '*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:

`int isMatch(const char *s, const char *p)`

Some examples:
```
isMatch("aa","a") -> 0
isMatch("aa","aa") -> 1
isMatch("aaa","aa") -> 0
isMatch("aa", "a*") -> 1
isMatch("aa", ".*") -> 1
isMatch("ab", ".*") -> 1
isMatch("aab", "c*a*b") -> 1
```
Return 0 / 1 ( 0 for false, 1 for true ) for this problem

## Hint 1

It might seem deceptively easy even you know the general idea, but programming it correctly with
all the details require careful thought.

Think carefully how you would do matching of ''.
Please note that '' in regular expression is different from wildcard matching,
as we match the previous character 0 or more times. But, how many times?
If you are stuck, recursion is your friend.

This problem is a tricky one. Due to the huge number of edge cases,
many people would write lengthy code and have numerous bugs on their first try.
Try your best getting your code correct first,
then refactor mercilessly to as clean and concise as possible!

## Solution Approach

![](http://i.imgur.com/8WGmiSd.png)

This looks just like a straight forward string matching, isn't it? Couldn't we just match the pattern and the input string character by character? The question is, how to match a '*' ?

A natural way is to use a greedy approach; that is, we attempt to match the previous character as many as we can. Does this work? Let us look at some examples.
```
s = "abbbc"
p = "ab*c"
```
Assume we have matched the first 'a' on both s and p. When we see `b*` in p, we skip all b's in s. Since the last 'c' matches on both side, they both match.
```
s = "ac"
p = "ab*c"
```
After the first 'a', we see that there is no b's to skip for `b*`. We match the last 'c' on both side and conclude that they both match.

It seems that being greedy is good. But how about this case?
```
s = "abbc"
p = "ab*bbc"
```
When we see `b*` in p, we would have skip all b's in s. They both should match, but we have no more b's to match. Therefore, the greedy approach fails in the above case.

One might be tempted to think of a quick workaround. How about counting the number of consecutive b's in s? If it is smaller or equal to the number of consecutive b's after "b*" in p, we conclude they both match and continue from there. For the opposite, we conclude there is not a match.

This seem to solve the above problem, but how about this case:
```
s = "abcbcd" 
p = "a.*c.*d"
```
Here, `.*` in p means repeat '.' 0 or more times. Since '.' can match any character, it is not clear how many times '.' should be repeated. Should the 'c' in p matches the first or second 'c' in s? Unfortunately, there is no way to tell without using some kind of exhaustive search.

We need some kind of backtracking mechanism such that when a matching fails, we return to the last successful matching state and attempt to match more characters in s with '*'. This approach leads naturally to recursion.

The recursion mainly breaks down elegantly to the following two cases:

If the next character of p is NOT `*`, then it must match the current character of s. Continue pattern matching with the next character of both s and p.
If the next character of p is `*`, then we do a brute force exhaustive matching of 0, 1, or more repeats of current character of p Until we could not match any more characters.

You would need to consider the base case carefully too. That would be left as an exercise to the reader. :)


## Solution
### Editorial
```cpp
int Solution::isMatch(const string A, const string B) {
    vector<vector<bool>> dp(A.length() + 2, vector<bool>(B.length() + 2, false));
    dp[0][0] = dp[1][1] = true;
    for (int i = 2; i < dp[0].size(); i++) {
        if (B[i - 2] == '*')
            dp[1][i] = dp[1][i - 2];
    }
    for (int i = 2; i < dp.size(); i++) {
        for (int j = 2; j < dp[0].size(); j++) {
            if (B[j - 2] == A[i - 2] || B[j - 2] == '.')
                dp[i][j] = dp[i - 1][j - 1];
            else if (B[j - 2] == '*') {
                dp[i][j] = dp[i][j - 2];
                if (B[j - 3] == A[i - 2])
                    dp[i][j] = dp[i][j] || dp[i - 1][j];
                if (B[j - 3] == '.')
                    dp[i][j] = true;
            }
        }
    }
    return dp[A.length() + 1][B.length() + 1];
}
```

### Mine
```cpp
int Solution::isMatch(const string A, const string B) {
    int i = A.size() - 1;
    int j = B.size() - 1;
    while (i >= 0 && j >= 0) {
        if (B[j] =='*') {
            j--;
            while (A[i] == B[j] || B[j] =='.') {
                i--;
                if (i < 0) return 1;
            }
            j--;
            if (j < 0) return 0;
        }
        if (A[i] == B[j] || B[j] =='.') {
            i--;
            j--;
        } else
            return 0;
    }
    if (i >= 0 && j < 0) return 0;
    if (i < 0 && j >= 0) return 0;
    return 1;
}
```

## Asked in

* Facebook
* Microsoft
* Google

