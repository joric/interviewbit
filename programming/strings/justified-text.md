# Justified Text

https://www.interviewbit.com/problems/justified-text



This problem is more of simulation. Take care of some of corner cases like space distribution in different lines.

Try to write an elegant solution.


1) A line other than the last line might contain only one word. What should you do in this case?

In this case, that line should be left-justified.

2) Have you noticed that the last line is an exception in terms of spaces?

This is more of a simulation problem. The more elegant your code, the less chances of it being bug prone,

and more marks in the interview.

Give a lot of thought to the structure of the code before you start coding.

## Solution

```cpp

/* editorial */

class Solution {
  public:
    vector<string> fullJustify(vector<string> &words, int L) {
        vector<string> res;
        int k = 0, l = 0;
        for (int i = 0; i < words.size(); i += k) {
            for (k = l = 0; i + k < words.size() && l + words[i + k].size() <= L - k; k++) {
                l += words[i + k].size();
            }
            string tmp = words[i];
            for (int j = 0; j < k - 1; j++) {
                if (i + k >= words.size())
                    tmp += " ";
                else
                    tmp += string((L - l) / (k - 1) + (j < (L - l) % (k - 1)), ' ');
                tmp += words[i + j + 1];
            }
            tmp += string(L - tmp.size(), ' ');
            res.push_back(tmp);
        }
        return res;
    }
};

/* another solution */

vector<string> Solution::fullJustify(vector<string> &A, int B) {
    vector<string> result;
    short int k = 0, ls = 0;
    for (short int i = 0; i < A.size(); i += k) {
        k = ls = 0;
        while (i + k < A.size() && ls + k + A[i + k].size() <= B) {
            ls += A[i + k].size();
            ++k;
        }
        string tmp = A[i];
        for (int j = 0; j < k - 1; j++) {
            if (i + k >= A.size())
                tmp += " ";
            else
                tmp += string((B - ls) / (k - 1) + (j < (B - ls) % (k - 1)), ' ');
            tmp += A[i + j + 1];
        }
        tmp += string(B - tmp.size(), ' ');
        result.emplace_back(tmp);
    }
    return result;
}
```