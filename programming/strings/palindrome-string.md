# Palindrome String

https://www.interviewbit.com/problems/palindrome-string

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

Example:

`A man, a plan, a canal: Panama` is a palindrome.

`race a car` is not a palindrome.

Return 0 / 1 ( 0 for false, 1 for true ) for this problem

## Hint 1

Maintain 2 pointers, one from the begining and one from the end of the string. How to handle the corner cases?


## Solution Approach

This is a fairly simple question.

You need to maintain 2 pointers, one from the beginning and one from the end.

At every iteration, after skipping the non alphanumeric characters, both the characters should match.

Have you considered empty strings?

Empty strings are palindromes. This is however, a nice question for clarification from the interviewer.

Note:

1. Are you correctly skipping the non alphanumeric characters?

2. Are you correctly handling whitespaces?


## Solution

```cpp
int Solution::isPalindrome(string A) {
    for (int i=0, j=A.length()-1; i<j; i++,j--) {
        for(;!isalnum(A[i]) && i<j; i++);
        for(;!isalnum(A[j]) && i<j; j--);
        if (tolower(A[i])!=tolower(A[j]))
            return 0;
    }
    return 1;
}

int Solution::isPalindrome(string A) {
    int i = 0, j = A.length()-1;
    while (i<j) {
        while (!isalnum(A[i]) && i<j)
            i++;
        while (!isalnum(A[j]) && i<j)
            j--;
        if (tolower(A[i])!=tolower(A[j]))
            return 0;
        i++;
        j--;
    }
    return 1;
}

int Solution::isPalindrome(string A) {
    string B;
    for (int i=0; i<A.size(); i++)
        if (isalnum(A[i]))
            B += tolower(A[i]);
    for (int i=0, n=B.length(); i<=n/2; i++)
        if (B[i]!=B[n-i-1])
            return 0;
    return 1;
}

```