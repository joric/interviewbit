# Longest Common Prefix

https://www.interviewbit.com/problems/longest-common-prefix/

Write a function to find the longest common prefix string amongst an array of strings.

Longest common prefix for a pair of strings S1 and S2 is the longest string S which is the prefix of both S1 and S2.

As an example, longest common prefix of "abcdefgh" and "abcefgh" is "abc".

Given the array of strings, you need to find the longest S which is the prefix of ALL the strings in the array.

Example:

Given the array as:
```
[
  "abcdefgh",
  "aefghijk",
  "abcefgh"
]
```
The answer would be "a".

## Hint 1

How about comparing two strings at a time to get their common prefix? Can this thing be used in order to generalize for all the strings?

## Solution Approach

Note: the prefix has to be the prefix of ALL the strings.

So, you can pick any random string from the array and start checking its characters from the beginning in order to see if they can be a part of the common substring.

## Solution

### Editorial
```cpp
string Solution::longestCommonPrefix(vector<string> &strs) {
    if (strs.size() == 0) return "";
    string ans = "";
    for (int i = 0; i < strs[0].length(); i++) {
        // checking if character i qualifies to be in the answer. 
        bool isQualified = true; 
        for (int j = 1; j < strs.size(); j++) {
            if (i >= strs[j].length() || strs[j][i] != strs[0][i]) {
                isQualified = false;
                break;
            }
        }
        if (!isQualified) break;
        ans.push_back(strs[0][i]);
    }
    return ans;
}
```
### My

```cpp
string Solution::longestCommonPrefix(vector<string> &A) {
    int res = 0, n = A.size();
    if (n==1)
        return A[0];
        
    string & s1 = A[0];

    for (int i=0; i<s1.size(); i++) {
        for (int j=1; j<n; j++) {
            string & s2 = A[j];
            if (s2.size()<=i || s1[i]!=s2[i])
                return s1.substr(0, res);
        }
        res++;
    }
    
    return s1.substr(0, res);
}
```

