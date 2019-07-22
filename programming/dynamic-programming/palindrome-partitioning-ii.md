# Palindrome Partitioning II

https://www.interviewbit.com/problems/palindrome-partitioning-ii/

Given a string s, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

Example:
```
Given 
s = "aab",
Return 1 since the palindrome partitioning ["aa","b"] could be produced using 1 cut.
```

## Hint 1

Firstly, we should be able to answer if substring [i,i+1,....j] is palindrome or not in O(1) with pre-computation of O(n^2).

Now try to come up with some DP state which can find minimum cut using above data.

## Solution Approach

Like other DP problems, we first look at the bruteforce approach. 
We look at the substrings starting with the index 0, and then figure out the minCut for remaining string. We choose the minimum minCut of the possibilities and our answer becomes min + 1.

The code would go something like this: 

```cpp
int minCut(string s) {
    // BASE CASES
    int minimum = infinity; 
    for (int index = 0; index < s.length(); index++) {
         if (s[0..index] is palindrome) minimum = min(minimum, minCut(s[index+1...]))
    }
    return minimum + 1;
 }
```
Now note that the string s will always be some suffix of original S. Which means only length(S) number of possibilities.

## Solution

### Fastest
```cpp
int Solution::minCut(string A) {
    int n = A.size();
    vector<int> cut(n+1, 0);  // number of cuts for the first k characters
    for (int i = 0; i <= n; i++) cut[i] = i-1;
    for (int i = 0; i < n; i++) {
        for (int j = 0; i-j >= 0 && i+j < n && A[i-j]==A[i+j] ; j++) // odd length palindrome
            cut[i+j+1] = min(cut[i+j+1],1+cut[i-j]);

        for (int j = 1; i-j+1 >= 0 && i+j < n && A[i-j+1] == A[i+j]; j++) // even length palindrome
            cut[i+j+1] = min(cut[i+j+1],1+cut[i-j+1]);
    }
    return cut[n];
}

```

### Lightweight
```cpp
bool isPalindrome(string A) {
    int left = 0;
    int right = A.size()-1;
    while(left < right) {
        if(A[left] != A[right]) {
            return 0;
        }
        left++;
        right--;
    }
    return 1;
}

int Solution::minCut(string A) {
    int n = A.size();
    vector<int> result(n+1);
    result[n] = -1;
    for(int i=n-1;i>=0;i--) {
        result[i] = n-i-1;
        for(int j=i;j<n;j++) {
            if(isPalindrome(A.substr(i, j-i+1))) {
                result[i] = min(result[i], 1 + result[j+1]);
            }
        }
    }
    return result[0];
}
```

## Asked in
* Amazon
* Google
