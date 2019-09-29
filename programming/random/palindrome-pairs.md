# Palindrome Pairs

https://www.interviewbit.com/problems/palindrome-pairs/


Given a list of unique words A, find all pairs of distinct indices (i,j) in the given list such that concatenation of the two words, i.e. A[i] + A[j] is a palindrome.

Note: A string is a palindrome if it reads the same backward as forward.

### Input Format

The only argument given is the integer array A.

### Output Format

Return the list of all pairs of distinct indices such that concatenation of the two words, i.e. A[i] + A[j] is a palindrome. 
You can return the list in any order.

### Constraints

* 1 <= length of the list A <= 1000
* 100 <= lenght of words in list A <= 100

### For Example
```
Input 1:
    A = ["abcd", "dcba", "lls", "s", "sssll"]
Output 1:
    [ [0,1], [1,0], [3,2], [2,4] ] 

Input 2:
    A = ["abc", "sa", "xy", "as" ]
Output 2:
    [ [1,3], [3,1] ]
```

## Hint1
Given: No duplicated string in the list.

Let N = length of the list.

let K = max length of the word in the list.

Approach 1:

Run two loops one for i and one for j and check if concatenation of A[i] and A[j] i.e. A[i]+A[j] is palindrome.
Time Complexity= KN^2

This will be sufficient for N=100 but for higher N it will lead to TLE.
Think of a more efficient approach.

## Hint2

Given: No duplicated string in the list.

Let N = length of the list.

let K = max length of the word in the list.

Approach 2(Optimized):

Traverse the array, build map. Key is the reversed string, value is index in array (0 based)

Main logic part. Partition the word into left and right, and see

if there exists a candidate in map equals the left side of current word, and right side of current word is palindrome, so concatenate(current word, candidate) forms a pair: left | right | candidate.
same for checking the right side of current word: candidate | left | right.

## Solution
### Editorial
```cpp
bool isPalindrome(string &str){
    if(str.empty())return true;
         int i = 0;
         int j = str.size() - 1;

         while(i < j)
             if(str[i++] != str[j--]) return false;
         return true;
}

vector<vector<int>> palindromePairs(vector<string>& words) {
         unordered_map<string, int> dict;
         vector<vector<int>> ans;
         // build dictionary
         for(int i = 0; i < words.size(); i++) {
             string key = words[i];
             reverse(key.begin(), key.end());
             dict[key] = i;
         }
         // edge case: if empty string "" exists, find all palindromes to become pairs ("", self)
         if(dict.find("")!=dict.end()){
             for(int i = 0; i < words.size(); i++){
                 if(i == dict[""]) continue;
                 if(isPalindrome(words[i])) ans.push_back({dict[""], i}); // 1) if self is palindrome, here ans covers concatenate("", self)
             }
         }

         for(int i = 0; i < words.size(); i++) {
             for(int j = 0; j < words[i].size(); j++) {
                 string left = words[i].substr(0, j);
                 string right = words[i].substr(j, words[i].size() - j);

                 if(dict.find(left) != dict.end()&& isPalindrome(right) && dict[left] != i) {
                     ans.push_back({i, dict[left]});     // 2) when j = 0, left = "", right = self, so here covers concatenate(self, "")
                 }

                 if(dict.find(right) != dict.end() && isPalindrome(left) && dict[right] != i) {
                     ans.push_back({dict[right], i});
                 }
             }
         }

         return ans;
}


vector<vector<int> > Solution::solve(vector<string> &A) {
return palindromePairs(A);
}

```

### Fastest
```cpp
bool isPali(string& str,int start,int end)
    {
        int j=start,k=end-1;
        while(j<=k)
        {
            if(str[j]==str[k])
            {
                j++;
                k--;
            }
            else
                return false;
        }
        return true;
    }
vector<vector<int> > Solution::solve(vector<string> &words) 
{
    
    
        unordered_map<string,int> dict;
        int size=words.size();
        for(int i=0;i<size;i++)
            dict[words[i]]=i;
        vector<vector<int>> re;
        for(int i=0;i<size;i++)
        {
            string temp1=words[i];
            reverse(temp1.begin(),temp1.end());
            auto it=dict.find(temp1);
            if(it!=dict.end()&& it->second!=i)
            {
                vector<int> v{it->second,i};
                re.push_back(v);
            }    
            for(int j=1;j<=words[i].size();j++)
            {
                if(isPali(words[i],0,j))
                {
                    string temp=words[i].substr(j,words[i].size()-j);
                    reverse(temp.begin(),temp.end());
                    auto itr=dict.find(temp);
                    if(itr!=dict.end()&& itr->second!=i)
                    {
                        vector<int> v{itr->second,i};
                        re.push_back(v);
                    }
                }
                if(isPali(words[i],words[i].size()-j,words[i].size()))
                {
                    string temp=words[i].substr(0,words[i].size()-j);
                    reverse(temp.begin(),temp.end());
                    auto itr=dict.find(temp);
                    if(itr!=dict.end()&& itr->second!=i)
                    {
                        vector<int> v{i,itr->second};
                        re.push_back(v);
                    }
                }
            }
        }
        
        return re;
    
}

```

### Lightweight
```cpp
bool isPalin(string s)
{
    int n=s.length()-1;
    for(int i=0; i<n; i++, n--)
        if(s[i]!=s[n])
            return false;
    return true;
}
vector<vector<int> > Solution::solve(vector<string> &arr)
{
    vector<vector<int>>ans;
    vector<int> row;
    for(int i=0; i<arr.size(); i++)
    {
        for(int j=0; j<arr.size(); j++)
        {
            if(i!=j)
            {
                if(isPalin(arr[i]+arr[j]))
                {
                    row.push_back(i);
                    row.push_back(j);
                    ans.push_back(row);
                    row.clear();
                }
            }
        }
    }
    return ans;
}

```

### Python
```python
class Solution:
    def solve(self, words):
        if not words: return [[]]
        lookup = {w: i for i,w in enumerate(words)}
        res = []
        for i, w in enumerate(words):
            for j in range(len(w)+1):
                pre, suf = w[:j], w[j:]
                if pre==pre[::-1] and suf[::-1]!=w and suf[::-1] in lookup:
                    res.append([lookup[suf[::-1]], i])
                if suf==suf[::-1] and pre[::-1]!=w and pre[::-1] in lookup and j!=len(w):
                    res.append([i, lookup[pre[::-1]]])
        return res
```

## References
* https://leetcode.com/problems/palindrome-pairs/

