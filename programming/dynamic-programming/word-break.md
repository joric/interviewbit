# Word Break

https://www.interviewbit.com/problems/word-break/

Given a string s and a dictionary of words dict, determine if s can be segmented
into a space-separated sequence of one or more dictionary words.

For example, given

```
s = "myinterviewtrainer",
dict = ["trainer", "my", "interview"].
```

Return 1 ( corresponding to true ) because "myinterviewtrainer" can be segmented as "my interview trainer".

Return 0 / 1 ( 0 for false, 1 for true ) for this problem


## Hint 1

Can you calculate the answer if you know which all substring of the given string are there in the dictionary?

Think of DP.


## Solution Approach

Lets again look at the bruteforce solution. 
You start exploring every substring from the start of the string, and check if its in the dictionary. If it is, then you check if it is possible to form rest of the string using the dictionary words. If yes, my answer is true. If none of the substrings qualify, then our answer is false.

So something like this :
```
bool wordBreak(int index, string &s, unordered_set<string> &dict) {
                // BASE CASES
    
    bool result = false;
    // try to construct all substrings.
    for (int i = index; i < s.length(); i++) {
        substring = *the substring s[index..i] with i inclusive*
        if (dict contains substring) {
            result |= wordBreak(i + 1, s, dict); // If this is true, we are done. 
        }
    }
    return result;
}
```
This solution itself is exponential. However, do note that we are doing a lot of repetitive work. 
Do note, that index in the wordBreak function call can only take s.length() number of values [0, s.length]. What if we stored the result of the function somehow and did not process it everytime the function is called ?

## Solution

### Editorial
```cpp
unordered_set<string> dict;
vector<bool> dp;
int Solution::wordBreak(string A, vector<string> &B) {
    dict.clear();
    dp.clear();
    dp.resize(A.size() + 1, 0);
    for (auto it : B) dict.insert(it);
    dp[A.size()] = 1;
    for (int i = A.size() - 1; i >= 0; i--) {
        string tmp = "";
        for (int j = i; j < A.size(); j++) {
            if (dp[i]) break;
            tmp += A[j];
            if (dict.find(tmp) != dict.end()) {
                dp[i] = dp[j + 1];
            }
        }
    }
    if (dp[0])
        return 1;
    return 0;
}
```

### Mine
```cpp
bool search(string A, vector<string> B){
    for(int i = 0; i < B.size(); i++){
        if(B[i] == A){
            return true;
        }
    }
    return false;
}

int Solution::wordBreak(string A, vector<string> &B) {
    int n = A.size();
    vector<int> temp(n+1, 0);
    temp[n] = 1;
    
    for(int i = n-1; i >= 0; i--){
        for(int j = i; j < n; j++){
            string s = A.substr(i, j-i+1);
            if(search(s, B) && temp[j+1] == 1){
                temp[i] = 1;
                break;
            }
        }
    }
    return temp[0];
}
```

## Asked in
* IBM
* Google
