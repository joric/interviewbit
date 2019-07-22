# Regular Expression Match

https://www.interviewbit.com/problems/regular-expression-match/


Implement wildcard pattern matching with support for '?' and '*'.

1. '?' : Matches any single character.
2. '*' : Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:

`int isMatch(const char *s, const char *p)`

Examples :

```
isMatch("aa","a") -> 0
isMatch("aa","aa") -> 1
isMatch("aaa","aa") -> 0
isMatch("aa", "*") -> 1
isMatch("aa", "a*") -> 1
isMatch("ab", "?*") -> 1
isMatch("aab", "c*a*b") -> 0
```

Return 1/0 for this problem.

## Hint 1
Think DP.

Can you do something with DP to match prefix of first string with prefix of another string.

## Solution Approach

Think about the bruteforce solution. 
When you encounter '', you would try to call the same isMatch function in the following manner: 
If p[0] == '', then isMatch(s, p) is true if isMatch(s+1, p) is true OR isMatch(s, p+1) is true. 

else if p[0] is not '*' and the characters s[0] and p[0] match ( or p[0] is '?' ), then isMatch(s,p) is true only if isMatch(s+1, p+1) is true. 

If the characters don't match isMatch(s, p) is false. This approach is exponential. Think why. 

Lets see how we can make this better. Note that isMatch function can only be called with suffixes of s and p. As such, there could only be length(s) * length(p) unique calls to isMatch. Lets just memoize the result of the calls so we only do processing for unique calls. This makes the time and space complexity O(len(s) * len(p)).

There could be ways of optimizing the approach rejecting certain suffixes without processing them. For example, if len(non star characters in p) > len(s), then we can return false without checking anything.


## Solution

### Editorial
```cpp
int Solution::isMatch(const string A, const string B) {
    int p = B.size();
    int q = A.size();
    int i = 0, j = 0;
    int iIndex = -1, jIndex = -1;
    while (i < q) {
        if (A[i] == B[j] || (j < p && B[j] == '?')) {
            i++;
            j++;
        } else if (j < p && B[j] == '*') {
            iIndex = i;
            jIndex = j;
            j++;
        } else if (jIndex != -1) {
            i = iIndex + 1;
            j = jIndex + 1;
            iIndex++;
        } else
            return 0;
    }
    while (j < p && B[j] == '*')
        j++;
    if (j == p)
        return 1;
    return 0;
}

```

### Lightweight
```cpp
int Solution::isMatch(const string s, const string p) {
    int n = s.size(), m = p.size();
    int star = -1, prev_start = 0;
    int i = 0, j = 0;

    for (; i < n;) {
        if (s[i] == p[j] || p[j] == '?') {
            i++, j++;
        } else if (p[j] == '*') {
            star = j++;
            prev_start = i;
        } else if (star != -1) {
            j = star;
            i = ++prev_start;
        } else {
            return false;
        }
    }

    while (j < m && p[j] == '*')
        j++;

    return (j == m);
}
```

### Mine
```cpp

int match(string s, int i, string p, int j){
    
    if(i >= s.size() && j >= p.size()){
        return 1;
    }
    
    if(i == s.size() && j < p.size()){
        for(int t = j; t < p.size(); t++){
            if(p[t] != '*'){
                return 0;
            }
        }
        return 1;
    }
    
    if(s[i] == p[j] || p[j] == '?'){
        return match(s, i+1, p, j+1);
    }
    
    if(p[j] == '*'){
        return max(match(s, i+1, p, j), match(s, i, p, j+1));
    }
    
}

int Solution::isMatch(const string s, const string p) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
    
    int pat = p.size();
    int str = s.size();
    
    int i = 0, j = 0;
    int iIndex = -1, jIndex = -1;
    
    while(i < str){
        if(s[i] == p[j] || (j < pat && p[j] == '?')){
            i++;
            j++;
        }
        else if(p[j] == '*' && j < pat){
            jIndex = j;
            iIndex = i;
            j++;
        }
        else if(jIndex != -1){
            j = jIndex + 1;
            i = iIndex + 1;
            iIndex++;
        }
        else{
            return 0;
        }
        
    }
    
    while(j < pat && p[j] == '*'){
        j++;
    }
    
    if(j == pat){
        return 1;
    }
    
    return 0;

}
```
## Asked in

* Facebook
* Microsoft


