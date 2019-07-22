# Substring Concatenation

https://www.interviewbit.com/problems/substring-concatenation



You are given a string, S, and a list of words, L, that are all of the same length.

Find all starting indices of substring(s) in S that is a concatenation of each word in L exactly
once and without any intervening characters.

Example :

S: "barfoothefoobarman"
L: ["foo", "bar"]
You should return the indices: [0,9].
(order does not matter).



You know that every word in L is of same length( say x). Let the number of words in L be n.

You need to check if every segment of length n*x in our main word consist of some permutation of all the words given in the list.

If you can do that for one segment you can just slide using two pointer and do it for all segments. For a single segment you can use hashing. How?



Think of the bruteforce solution. 
Lets say the size of every word is wsize and number of words is lsize. 
You start at every index i. Look at every lsize number of chunks of size wsize and note down the words. Then match the set of words encountered to the set of words expected.

Now, lets look at ways we can optimize this. 
Right now, to match words, we do it letter by letter. How about hashing the words ? 
With hashing, hash(w1) + hash(w2) = hash(w2) + hash(w1). 
In short, when adding the hashes, the order of words does not matter. 
Can we optimize the matching of all the words encountered using that ? Can we use sliding pointers to move to index i + wsize from i ?



## Solution

```cpp

/* editorial */

#if 0
vector<int> Solution::findSubstring(string A, const vector<string> &B) {

    vector<int> ans;
    int size = B.size(), len1 = A.length(), len2 = 0, len = 0;
    if (size == 0 || len1 == 0)
        return ans;
    len = B[0].length(); //length of 1 string
    len2 = size * len;   //length of required substring
    if (len2 > len1)
        return ans;
    unordered_map<string, int> m;
    for (int i = 0; i < size; i++)
        m[B[i]]++;

    for (int i = 0; i <= len1 - len2; i++) {
        //start with each index and consider a substring of length len2
        unordered_map<string, int> m1;
        m1 = m;
        int count = 0;
        for (int j = i; j < i + len2; j += len) {
            string temp = A.substr(j, len);
            if (m1.find(temp) != m1.end() && m1[temp] > 0)
                m1[temp]--;
            else
                break;
            if (m1[temp] == 0)
                count++;
        }
        if (count == m.size())
            ans.push_back(i);
        m1.clear();
    }
    return ans;
}

/* another solution */

vector<int> Solution::findSubstring(string A, const vector<string> &B) {
    int noOfWords = B.size();
    vector<int> res;
    if (A.size() == 0 || B.size() == 0) {
        return res;
    }

    int wordSize = B[0].size();
    unordered_map<string, int> hash;

    for (const auto &b : B)
        ++hash[b];

    auto i = 0;
    while ((i + wordSize * noOfWords - 1) < A.size()) {
        unordered_map<string, int> tempHash;
        auto j = 0;
        while (j < A.size()) {
            string word = A.substr(i + j * wordSize, wordSize);
            if (hash.find(word) == hash.end()) {
                break;
            } else {
                if (++tempHash[word] > hash[word]) {
                    break;
                }
                ++j;
            }

            if (j == noOfWords)
                res.emplace_back(i);
        }
        ++i;
    }

    return res;
}

#endif

/* another solution */

#include <bits/stdc++.h>
using namespace std;
struct Solution {
    vector<int> findSubstring(string a, const vector<string> &b);
};

#if 0
vector<int> Solution::findSubstring(string a, const vector<string> &b) {
    unordered_map<string, int> word;
    for (auto i : b)
        word[i]++;

    int n = a.length();
    int len = b.size();
    int sz = b[0].length();

    vector<int> ans;
    for (int i = 0; i < n - len * sz + 1; i++) {
        unordered_map<string, int> s;
        int j = 0;
        for (; j < len; j++) {
            string words = a.substr(i + j * sz, sz);
            cout << i << " " << j << " " << words << endl;
            if (word[words]) {
                s[words]++;
                if (s[words] > word[words])
                    break;
            } else
                break;
        }
        if (j == len)
            ans.push_back(i);
    }
    return ans;
}
#endif

int hashfunc(string str) {
    unsigned long int hash1 = 3853;
    int n;
    cout << str << " hashed ";//comment it
    for (int i = 0; i < str.size(); i++) {
        hash1 += str[i];
        hash1 += (hash1 << 10);
        hash1 ^= (hash1 >> 6);
    }
    hash1 += (hash1 << 3);
    hash1 ^= (hash1 >> 11);
    hash1 += (hash1 << 15);

    return hash1;
}

vector<int> Solution::findSubstring(string A, const vector<string> &B) {
    cout << A << endl;

    int n = B.size() * B[0].size();
    int m = B[0].size();
    vector<int> ans;
    map<string, int> mp, mp2;

    if (n > A.size())
        return ans;

    unsigned int hash1 = 0, hash2;

    for (int i = 0; i < B.size(); i++) {
        hash1 += hashfunc(B[i]);
    }

    cout << "h1 " << hash1 << endl;//comment it

    int flag;
    for (int i = 0; i <= A.size() - n; i++) {
        flag = 1;
        hash2 = 0;
        for (int j = i; j < i + n; j = j + m) {
            string str = A.substr(j, m);
            hash2 += hashfunc(str);
        }
        cout << "h2 " << hash2 << endl;//comment it
        if (hash2 == hash1)
            ans.push_back(i);
    }
    return ans;
}

int main() {
    string a = "barfoothefoobarman";
    vector<string> b = { "foo", "bar" };

    for (auto x : Solution().findSubstring(a, b))
        cout << x << " ";
    cout << endl;
}
```