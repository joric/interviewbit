# Implement Strstr

https://www.interviewbit.com/problems/implement-strstr


Implement strStr().

 strstr - locate a substring ( needle ) in a string ( haystack ). 
Try not to use standard library string functions for this question.

Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Lets say M is length of haystack and N is length of needle. Then expected complexity here is O(N*M).


Let us first think about a simpler problem. How do you find out if 2 strings are equal?

Implementing strstr is just plain simple simulation.

Consider every index i for the answer. Find if the following 2 strings are equal:

1) Needle string and,

2) String haystack from index i with length the same as needle's length

Note that the complexity of this solution is O(M*N) where M is length of haystack and N is length of needle.

If you are feeling more adventurous, try solving it in O(M).

*Hint: KMP Algorithm**

## Solution

```cpp


class Solution {
public:
    int strStr(const string &haystack, const string &needle) {
            if (needle[0] == '\0') return 0;
        for (int i = 0; haystack[i] != '\0'; i++) {
            bool isMatched = true; 
            for (int j = 0; needle[j] != '\0'; j++) {
                // If remaining haystack length is less than needle's length, 
                // we know needle is not present in haystack.
                if (haystack[i + j] == 0) return -1;
                if (haystack[i + j] != needle[j]) {
                    isMatched = false;
                    break;
                }
            }
            if (isMatched) return i; // Match found
        }
        return -1;
    }
};

#if 0
int Solution::strStr(const string A, const string B) {
    return A.find(B);
}
#endif

```