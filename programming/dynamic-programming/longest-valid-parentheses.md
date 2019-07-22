# Longest valid Parentheses

https://www.interviewbit.com/problems/longest-valid-parentheses/

Given a string containing just the characters '(' and ')', find the length of the longest
valid (well-formed) parentheses substring.

For "(()", the longest valid parentheses substring is "()", which has length = 2.

Another example is ")()())", where the longest valid parentheses substring is "()()", which has length = 4.

## Hint 1

If you know the longest parenthesis ending at index i, can you use that to compute answer?

Try to simulate using stack and DP.

## Solution Approach

Lets construct longest[i] where longest[i] denotes the longest set of parenthesis ending at index i.

If s[i] is '(', set longest[i] to 0, because any string end with '(' cannot be a valid one.
Else if s[i] is ')' 
If s[i-1] is '(', longest[i] = longest[i-2] + 2 
Else if s[i-1] is ')' and s[i-longest[i-1]-1] == '(', longest[i] = longest[i-1] + 2 + longest[i-longest[i-1]-2]

## Solution

### Editorial
```cpp
int Solution::longestValidParentheses(string s) {
    if (s.length() <= 1) return 0;
    int curMax = 0;
    vector<int> longest(s.size(), 0);
    for (int i = 1; i < s.length(); i++) {
        if (s[i] == ')' && i - longest[i - 1] - 1 >= 0 && s[i - longest[i - 1] - 1] == '(') {
            longest[i] = longest[i - 1] + 2 + ((i - longest[i - 1] - 2 >= 0)
                ? longest[i - longest[i - 1] - 2]: 0);
            curMax = max(longest[i], curMax);
        }
    }
    return curMax;
}

```

### Mine
```cpp
int Solution::longestValidParentheses(string A) {
    int sol = 0, curr = 0, start = -1;
    int i = 0;
    stack<int> st;
    st.push(-1);
    while (i < A.size()) {
        if (A[i] == '(') {
            st.push(i);
        } else {
            st.pop();
            if (!st.empty()) {
                sol = max(i - st.top(), sol);
            } else {
                st.push(i);
            }
        }
        i++;
    }
    return sol;
}

```

## Asked in
* Google
