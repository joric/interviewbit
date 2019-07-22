# Multiply Strings

https://www.interviewbit.com/problems/multiply-strings


Given two numbers represented as strings, return multiplication of the numbers as a string.
 Note: The numbers can be arbitrarily large and are non-negative.
Note2: Your answer should not have leading zeroes. For example, 00 is not a valid answer. 
For example, 
given strings "12", "10", your answer should be "120".
NOTE: DO NOT USE BIG INTEGER LIBRARIES ( WHICH ARE AVAILABLE IN JAVA / PYTHON ). 
We will retroactively disqualify such submissions and the submissions will incur penalties.
## Solution

```cpp

/* editorial */

class Solution {
  public:
    std::string multiply(std::string num1, std::string num2) {

        int n1 = num1.size();
        int n2 = num2.size();
        if (n1 == 0 || n2 == 0)
            return "0";

        // will keep the result number in vector in reverse order
        std::vector<int> result(n1 + n2, 0);

        int i_n1 = 0; // index by num1
        int i_n2 = 0; // index by num2

        // go from right to left by num1
        for (int i = n1 - 1; i >= 0; i--) {
            int carrier = 0;
            int n1 = num1[i] - '0';
            i_n2 = 0;

            // go from right to left by num2
            for (int j = n2 - 1; j >= 0; j--) {
                int n2 = num2[j] - '0';

                int sum = n1 * n2 + result[i_n1 + i_n2] + carrier;
                carrier = sum / 10;
                result[i_n1 + i_n2] = sum % 10;

                i_n2++;
            }

            // store carrier in next cell
            if (carrier > 0)
                result[i_n1 + i_n2] += carrier;

            i_n1++;
        }

        // ignore '0's from the right
        int i = result.size() - 1;
        while (i >= 0 && result[i] == 0)
            i--;

        // if all were '0's - means either both or one of num1 or num2 were '0'
        if (i == -1)
            return "0";

        // generate the result string
        std::string s = "";
        while (i >= 0)
            s += std::to_string(result[i--]);

        return s;
    }
};

/* fastest */

string Solution::multiply(string A, string B) {
    if (A == "0" || B == "0")
        return "0";

    int aL = A.length(), bL = B.length();
    vector<int> result(aL + bL, 0);
    string res = "";

    for (auto i = aL - 1; i >= 0; --i) {
        for (auto j = bL - 1; j >= 0; --j) {
            result[i + j + 1] += (A[i] - '0') * (B[j] - '0');
        }
    }

    for (auto k = aL + bL - 1; k > 0; --k) {
        if (result[k] >= 10) {
            result[k - 1] += result[k] / 10;
            result[k] %= 10;
        }
    }

    int cnt = 0;
    for (auto l = 0; l < result.size(); ++l) {
        if (result[l] == 0 && l == cnt) //To omit the leading zeroes E.g. 00456723 will be 456723.
            ++cnt;
        else
            res += result[l] + '0'; //I was doing "-0" it was throwing internal error!
    }
    return res;
}

/* lightweight */

string Solution::multiply(string A, string B) {
    reverse(A.begin(), A.end());
    reverse(B.begin(), B.end());
    string res;
    res.resize(A.size() + B.size(), '0');
    for (int i = 0; i < A.size(); ++i) {
        int p = 0;
        for (int j = 0; j < B.size() || p; ++j) {
            int val = res[i + j] - '0';
            int mul = 0;
            if (j < B.size()) {
                mul = (B[j] - '0');
            }
            val += (A[i] - '0') * mul + p;
            p = val / 10;
            val %= 10;
            res[i + j] = (val + '0');
        }
    }
    while ((res.size() > 1) && (res[res.size() - 1] == '0'))
        res.pop_back();
    reverse(res.begin(), res.end());
    return res;
}

/* another solution */

string Solution::multiply(string A, string B) {
    if (A == "0" || B == "0")
        return "0";

    int aL = A.length(), bL = B.length();
    vector<int> result(aL + bL, 0);
    string res = "";

    for (auto i = aL - 1; i >= 0; --i) {
        for (auto j = bL - 1; j >= 0; --j) {
            result[i + j + 1] += (A[i] - '0') * (B[j] - '0');
        }
    }

    for (auto k = aL + bL - 1; k > 0; --k) {
        if (result[k] >= 10) {
            result[k - 1] += result[k] / 10;
            result[k] %= 10;
        }
    }

    int cnt = 0;
    for (auto l = 0; l < result.size(); ++l) {
        if (result[l] == 0 && l == cnt) //To omit the leading zeroes E.g. 00456723 will be 456723.
            ++cnt;
        else
            res += result[l] + '0'; //I was doing "-0" it was throwing internal error!
    }
    return res;
}
```