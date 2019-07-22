# Longest Substring Without Repeat

https://www.interviewbit.com/problems/longest-substring-without-repeat



Given a string, 
find the length of the longest substring without repeating characters.

Example:

The longest substring without repeating letters for "abcabcbb" is "abc", which the length is 3.

For "bbbbb" the longest substring is "b", with the length of 1.

## Hint 1

Think in terms of two pointers. 
If you encounter a repeating character, you won't be able to include the new character till
you exclude out the previous occurrence of the character. Which means your window needs to shrink
till your start character is pointing to the position next to previous occurrence of the character.


## Hint 2

All you need to do is use two pointers to keep a window with no repetitions of characters. Keep moving
the right pointer and if you encounter any repeating character start moving left pointer untill no character is repeated.

Also, note that the size of character set is small ( 128 at max ), so tracking the count of characters
in the current set is fairly easy using hashing / array buckets.


## Solution

```cpp


// my
int Solution::lengthOfLongestSubstring(string A) {
    unordered_map<char, int> m;
    int i = 0, j = 0, len = 0;
    while (j < A.size()) {
        if (m.count(A[j]) && m[A[j]] >= i)
            i = m[A[j]] + 1;
        len = max(len, j - i + 1);
        m[A[j]] = j;
        j++;
    }
    return len;
}


// editorial

int Solution::lengthOfLongestSubstring(string s) {
    int start = 0, end = 0;
    int longest = 0;

    // Hash which tracks the count of each character in the current window.
    // We need to make sure that for a solution, none of the
    // character count / hash value exceeds 1.
    int hash[260];
    memset(hash, 0, sizeof(hash));

    while (start <= end && end < s.length()) {
        hash[s[end]]++;
        if (hash[s[end]] > 1) {
            // pop stuff out of hash till the count becomes 1.
            while (start <= end && hash[s[end]] > 1) {
                hash[s[start]]--;
                start++;
            }
        }
        end++;
        longest = max(longest, end - start);
    }
    return longest;
}


int Solution::lengthOfLongestSubstring(string A) {
    unordered_map<char, int> m;
    int k = 0, res = 0;
    for (int i = 0; i < A.size(); i++) {
        if (m.find(A[i]) != m.end()) {
            i = m[A[i]];
            m.clear();
            res = max(res, k);
            k = 0;
        } else {
            m[A[i]] = i;
            k++;
        }
    }
    return max(res, k);
}

int Solution::lengthOfLongestSubstring(string A) {
    unordered_map<char, int> m;
    int count = 0;
    int ans = 0;
    auto size = A.size();
    int i = 0;
    while (i < size) {
        if (m.find(A[i]) == m.end()) {
            m[A[i]] = i;
            count++;
            i++;
        } else {
            i = m[A[i]] + 1;
            m.clear();
            ans = max(count, ans);
            count = 0;
        }
    }
    return max(count, ans);
}
```