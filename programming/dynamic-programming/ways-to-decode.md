# Ways To Decode

https://www.interviewbit.com/problems/ways-to-decode



## Hint 1

Try to compute answer for all prefixes of different lengths.

For computing number of ways for decoding string upto ith length you can use number of ways for decoding string upto (i-1)th or (i-2)th length.

Think DP.

Solution approach

Lets first look at the bruteforce solution. 
It only makes sense to look at 1 digit or 2 digit pairs ( as 3 digit sequence will be greater than 26 ).

So, when looking at the start of the string, we can either form a one digit code, and then look at the ways of forming the rest of the string of length L - 1, or we can form 2 digit code if its valid and add up the ways of decoding rest of the string of length L - 2.
This obviously is exponential.

The code would go something like the following :

int ways(string &s, int startIndex) {
    // BASE CASES
    int answer = 0;
    if (isValid(s[startIndex])) answer += ways(s, startIndex + 1);
    if (isValid(s[startIndex] + s[startIndex + 1])) answer += ways(s, startIndex + 2);

    return answer;
}


Note that in this case, startIndex can only take L number of values. Can you use the fact to store the result so that the function processing does not happen so many times ?

## Solution

```cpp

////////////////
// editorial

int Solution::numDecodings(string A) {
    int length = A.length();
    //check for corner cases such as string starting with 0 just return 0
    if (length == 0 || A[0] == '0')
        return 0;
    // dp array to memorize
    long long dp[length + 1];
    //initiliaze array to 0
    for (int i = 0; i <= length; i++)
        dp[i] = 0;
    // string with length 0 or 1 can be decoded in 1 ways
    dp[0] = 1;
    dp[1] = 1;
    //traverse the string
    for (int i = 2; i <= length; i++) {
        //if ith character is not zero then it can be split in two parts
        if (A[i - 1] != '0') {
            dp[i] += dp[i - 1];
        } else if (A[i - 2] == '0') //if this happens answer will be 0
            dp[i] = 0;
        //check if decoding two last character can produce meaning string
        if (A[i - 2] == '1' || (A[i - 2] == '2' && A[i - 1] < '7'))
            dp[i] += dp[i - 2];
    }
    //return asnwer
    return dp[length];
}

////////////////
// fastest

int Solution::numDecodings(string s) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
    if (s[0] == '0')
        return 0;
    int n = s.size();
    int dp[n + 1];
    dp[0] = 1;
    dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        int var = (s[i - 2] - '0') * 10 + (s[i - 1] - '0');
        if (s[i - 1] == '0') {
            if (var == 10 || var == 20)
                dp[i] = dp[i - 2];
            else
                return 0;

        } else {
            if (var > 26)
                dp[i] = dp[i - 1];
            else if (var >= 1 && var <= 9)
                dp[i] = dp[i - 1];
            else
                dp[i] = dp[i - 1] + dp[i - 2];
        }
    }
    return dp[n];
}

//////////////
// lightweight

int ways(const string &A, int s) {
    if (s == A.size())
        return 1;
    else if (A[s] == '0')
        return 0;
    else if (A[s] > '2')
        return ways(A, s + 1);
    else if (s + 1 >= A.size())
        return ways(A, s + 1);
    else if (A[s] == '1' || (A[s] == '2' && A[s + 1] < '7'))
        return ways(A, s + 1) + ways(A, s + 2);
    else
        return ways(A, s + 1);
}

int Solution::numDecodings(string A) {
    return ways(A, 0);
}

/////////////////
// my

int Solution::numDecodings(string A) {
    if (A.size() == 0) {
        return 0;
    } else if (A[0] == '0') {
        return 0;
    } else if (A.size() == 1) {
        return 1;
    }

    vector<int> temp(A.size() + 1);

    temp[0] = 1;
    temp[1] = 1;

    for (int i = 2; i < temp.size(); i++) {
        temp[i] = 0;

        if (A[i - 1] - '0' > 0) {
            temp[i] = temp[i - 1];
        }
        if (A[i - 1] == '0' && A[i - 2] > '2') {
            return 0;
        }
        if ((A[i - 2] - '0' < 2 && A[i - 2] - '0' > 0) || (A[i - 2] - '0' == 2 && A[i - 1] - '0' <= 6)) {
            temp[i] = temp[i] + temp[i - 2];
        }
    }

    return temp[temp.size() - 1];
}
```