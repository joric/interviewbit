# Palindromic Substrings Count

https://www.interviewbit.com/problems/palindromic-substrings-count/

Given a string Aconsisting of lowercase English alphabets. Your task is to find how many substrings of A are palindrome.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

Return the count of palindromic substrings.

Note: A string is palindrome if it reads the same from backward and forward.

### Input Format

The only argument given is string A.

### Output Format

Return the count of palindromic substrings.

### Constraints

1 <= length of the array <= 1000

### For Example
```
Input 1:
    A = "abab"
Output 1:
    6
    Explanation 1:
        6 palindromic substrings are:
        "a", "aba", "b", "bab", "a" and "b".

Input 2:
    A = "ababa"
Output 2:
    9
```

## Hint 1

Trivial approach: For each substring chechk whether it is palindrome or not.
Time complexity : O(N^2)

We can do better?
there are two substring A[l,r], B[l-x,r+x]
if A is not palindrome then we don’t need to check whether B is palindrome,else we have to check further for B.

## Solution Approach

Let N be the length of the string. The middle of the palindrome could be in one of 2N - 1 positions: either at letter or between two letters.

For each center, let’s count all the palindromes that have this center. Notice that if [a, b] is a palindromic interval (meaning S[a], S[a+1], …, S[b] is a palindrome), then [a+1, b-1] is one too.

For each possible palindrome center, let’s expand our candidate palindrome on the interval [left, right] as long as we can. The condition for expanding is left >= 0 and right < N and S[left] == S[right]. That means we want to count a new palindrome S[left], S[left+1], …, S[right].

## Solution
### Editorial
```cpp
int countSubstrings(string s) {
    int ans=0;
    int n=s.size();
    for(int i=0; i<n; ++i){
        for(int k=i,j=i; k<n&&j>=0; ++k,--j)
            if(s[k]==s[j])
                    ++ans;
            else
                break;
        for(int k=i,j=i-1; k<n&&j>=0; ++k,--j)
            if(s[k]==s[j])
                    ++ans;
            else
                break;
    }
    return ans;
}

int Solution::solve(string A) {
    return countSubstrings(A);
}
```
### Fastest
```cpp
bool isPal(string& A, int s, int e)
{
    int i = s;
    int j = e;
    while (i < j) {
        if (A[i++] != A[j--])
            return false;
    }
    return true;
}
int naive(string& A)
{
    int res = 0;
    for (int i = 0; i < A.size()-1; ++i) {
        for (int j = i+1; j < A.size(); ++j) {
            if (isPal(A, i, j))
                res++;
        }
    }
    // add number of 1-char palindroms
    return res + A.length();
}
int getPals(string& A, int i) {
    int num = 0;
    const int n = A.length();
    
    // Even length palindrome
    int l = i-1, r = i;
    while (l >= 0 && r < n) {
        if (A[l] != A[r]) break;
        num++;
        l--; r++;
    }
    
    // Odd length palindrome
    l = i-1;
    r = i+1;
    while (l >= 0 && r < n) {
        if (A[l] != A[r]) break;
        num++;
        l--; r++;
    }
    return num;
}

int Solution::solve(string A) {
    //return naive(A);
    int res = 0;
    const int n = A.length();
    for (int i = 1; i < n; ++i) {
        res += getPals(A, i);
    }
    // add number of 1-char palindroms
    return res + n;
}
```

### Mine (python)
```python
class Solution:
    def solve(self, S):
        N = len(S)
        ans = 0
        for center in range(2*N - 1):
            left = center // 2
            right = left + center % 2
            while left >= 0 and right < N and S[left] == S[right]:
                ans += 1
                left -= 1
                right += 1
        return ans
```



