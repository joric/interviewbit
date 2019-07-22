# Generate All Parentheses II

https://www.interviewbit.com/problems/generate-all-parentheses-ii



Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses of length 2*n.

For example, given n = 3, a solution set is:

"((()))", "(()())", "(())()", "()(())", "()()()"
Make sure the returned list of strings are sorted.



You can try thinking of recursion such that at every step our solution is valid.

How to define this valid thing now?



Again, think recursion. 
At every step, you have 2 options: 
1) Add '(' to the string. 
2) Add ')' to the string. 
However, you need to make sure, that the number of closing brackets do not exceed
number of opening brackets at any point of time. 
Also, make sure that the number of opening brackets never exceeds n.


## Solution

```cpp

// lightweight

void brackets(vector<string> &st, int n, int open, int closed, string s) {
    if (closed > open) return;
    if (open + closed == n && open == closed)
        st.push_back(s);
    else if (open + closed < n) {
        brackets(st, n, open + 1, closed, s + '(');
        brackets(st, n, open, closed + 1, s + ')');
    }
}

vector<string> Solution::generateParenthesis(int n) {
    vector<string> res;
    brackets(res, 2 * n, 0, 0, "");
    return res;
}

////////////////////////

void backtracking(int n, int open, int close, string str, vector<string> &res) {
    if (close == n) {
        res.emplace_back(str);
        return;
    } else {
        if (open < n) {
            str += '(';
            backtracking(n, open + 1, close, str, res);
            str.pop_back();
        }
        if (open > close) {
            str += ')';
            backtracking(n, open, close + 1, str, res);
            str.pop_back();
        }
    }
}

vector<string> Solution::generateParenthesis(int A) {
    vector<string> res;

    if (A > 0)
        backtracking(A, 0, 0, "", res);
    return res;
}
```