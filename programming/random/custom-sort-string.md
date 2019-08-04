# Custom Sort String

https://www.interviewbit.com/problems/custom-sort-string

Given two string A and B, both consisting of lowercase letters. A consists of uniques lowercase English letter 
i.e. no characters occur twice in it.

A was sorted in some custom order previously. We want to permute the characters of B so that they match the order that A was sorted.

More specifically, if character x occurs before y in A, 
then x should occur before y in the returned string.
For the elements not present in A, append them at last in lexicographical order.

Return the permutation of string B that matches the above criteria.


### Input Format
```
The First argument given is String A.
The Second argument given is String B.
```

###Output Format

Return the permutation of string B that matches the given criteria.
### Constraints
```
1 <= length of the string B <= 100000
1 <= length of the string A <= 26
```
### For Example

```
Input 1:
    A = "cda"
    B = "abcd"
Output1:
    "cdab"
    Explanation 1:
        character 'c' should occur before 'a' and 'd'.
        character 'd' should occur before 'a' 
        character 'b' is not present in A therefore it is appended at the end.
Input 2:
    A = "erhdqo"
    B = "wwdybr"
Output2:
    "rdbwwy"
```
## Solution Approach

1. Loop through B, store the count of every character in a HashMap (key: character, value: count of character) .
2. Loop through A, check if it is present in HashMap, 
if so, put in output array that many times and remove the character from HashMap.
3. Sort the rest of the characters present in HashMap and put in output array.


## Solution

### Editorial
```cpp
string Solution::solve(string A, string B) {
    map<int,int> mp;
    string s;
    for(int i=0;i<B.length();i++){
        mp[B[i]] += 1;
    }
    for(int i=0;i<A.length();i++){
        if(mp[A[i]] > 0){
            for(int j = 0;j<mp[A[i]];j++)
            s+= A[i];
        }
        mp.erase(A[i]);
    }
    for(auto i:mp){
        for(int j=0;j<i.second;j++){
            s+= i.first;
        }
    }
    return s;
}

```

### Fastest
```cpp
string Solution::solve(string A, string B) {
    int buck[26] = {0};
    for (auto e : B)
        buck[e - 'a']++;
        
    string res;
    for (auto e : A) {
        if (buck[e - 'a'] > 0) {
            res.append(buck[e - 'a'], e);
            buck[e - 'a'] = 0;
        }
    }
    
    for (int i = 0; i < 26; ++i) {
        if (buck[i] > 0) {
            res.append(buck[i], 'a' + i);
        }
    }
    return res;
}
```

### Lightweight
```cpp
int ind[26];


bool comp( char c, char d){
    
    int a = c-'a', b = d-'a';
    if(ind[a] == -1 && ind[b] == -1 ){
        return a<b;
    }
    if( ind[a] == -1 ){
        return false;
    }
    if( ind[b] == -1 )
        return true;
        
    return ind[a]<ind[b];
}
string Solution::solve(string A, string B) {
    
    for(int i=0; i<26; i++) ind[i] = -1;
    for(int index = 0; index<A.length(); index++ ){
        ind[ A[index]-'a' ] = index;
    }
    
    sort(B.begin(), B.end(), comp);
    return B;
}
```


### Mine
```cpp
string Solution::solve(string A, string B) {
    unordered_map<char, int> m;
    string C, D;
    for (int i=0; i<A.size(); i++) m[ A[i] ] = i;
    for (char c:B) if (m.count(c)) C.push_back(c); else D.push_back(c);
    sort(C.begin(), C.end(), [&m](const char& a, const char& b) { return m[a]<m[b]; });
    sort(D.begin(), D.end());
    return C+D;
}

```
### Another Mine
```cpp
string Solution::solve(string A, string B) {
    vector<char> m(128);
    for (int i=0; i<A.size(); i++) m[ A[i] ] = i+1;
    sort(B.begin(), B.end(), [&m](const char& a, const char& b){
        return ( m[a] ? m[a] : a<<8) < ( m[b] ? m[b] : b<<8); });
    return B;
}
```
