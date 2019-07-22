# Word Break II

https://www.interviewbit.com/problems/word-break-ii/

Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

For example, given
```
s = "catsanddog",
dict = ["cat", "cats", "and", "sand", "dog"].
```
A solution is
```
[
  "cat sand dog", 
  "cats and dog"
]
```
Make sure the strings are sorted in your result.

## Hint 1

How would you go about writing a bruteforce solution?

You start exploring every substring from the start of the string, and check if its in the dictionary. If it is, then you check if it is possible to form rest of the string using the dictionary words. If yes, you append the current substring to all the substring possible from rest of the string. If none of the substrings qualify, then there are no sentences possible from this string.

Hint: Think dynamic programming

Do you think that there is a lot of repetitive work happening here? 

Can you use Dynamic programming here to reduce the repetitive work happening here?

## Solution Approach

Lets again look at the bruteforce solution. 
You start exploring every substring from the start of the string, and check if its in the dictionary. If it is, then you check if it is possible to form rest of the string using the dictionary words. If yes, you append the current substring to all the substring possible from rest of the string. If none of the substrings qualify, then there are no sentences possible from this string. .

So something like this :

```cpp
vector<string> wordBreak(int index, string &s, unordered_set<string> &dict) {
    // BASE CASES
    vector<string> sentences;
    // try to construct all substrings.
    for (int i = index; i < s.length(); i++) {
        substring = *the substring s[index..i] with i inclusive*
            if (dict contains substring) {
                vector<string> ret = wordBreak(i + 1, s, dict);
                foreach (sentence in ret) {
                    sentences.push_back(substring + " " + sentence);
                }          
            }
    }
    return sentences;
}
```

This solution itself is exponential. However, do note that we are doing a lot of repetitive work. 
Do note, that index in the wordBreak function call can only take s.length() number of values [0, s.length]. What if we stored the result of the function somehow and did not process it everytime the function is called ?

## Solution

### Editorial
```cpp
vector<string> Solution::wordBreak(string s, vector<string> &dictV) {
    unordered_set<string> dict(dictV.begin(), dictV.end());
    vector<vector<string>> words(s.length() + 1, vector<string>(0));

    // initialize the valid values
    words[s.length()].push_back("");

    // generate solutions from the end
    for (int i = s.length() - 1; i >= 0; i--) {
        vector<string> values;
        for (int j = i + 1; j <= s.length(); j++) {
            if (dict.find(s.substr(i, j - i)) != dict.end()) {
                for (int k = 0; k < words[j].size(); k++) {
                    values.push_back(s.substr(i, j - i) + (words[j][k].empty() ? "" : " ") + words[j][k]);
                }
            }
        }
        words[i] = values;
    }
    return words[0];
}
```
### Fastest
```cpp
vector<string> Solution::wordBreak(string A, vector<string> &B) {
    vector< vector<string> > dp(A.length());
    vector<string> B_new;
    sort(B.begin(),B.end());
    for(int i=0;i<B.size();i++)if(i==0 || B[i]!=B[i-1])B_new.push_back(B[i]);
    B=B_new;
    for(int i=0;i<A.length();i++){
        for(int j=0;j<B.size();j++){
            if((B[j].length() <= (i+1)) && (A.substr(i-B[j].length()+1,B[j].length()) == B[j])){
                if((i+1)==B[j].length())dp[i].push_back(B[j]);
                else{
                    for(int k=0;k<dp[i-B[j].length()].size();k++)dp[i].push_back(dp[i-B[j].length()][k] + " " + B[j]);
                }
            }
        }
    }
    sort(dp[A.length()-1].begin(),dp[A.length()-1].end());
    return dp[A.length()-1];
}
```

## Asked in

* IBM
* Google

