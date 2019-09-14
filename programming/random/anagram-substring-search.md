# Anagram Substring Search

https://www.interviewbit.com/problems/anagram-substring-search/


Given a string A and a string B.

Find and return the starting indices of the substrings of A which matches any of the anagrams of B.

Note: An anagram is a play on words created by rearranging the letters of the original word to make a new word or phrase

### Input Format

The arguments given are string A and string B.

### Output Format

Return the starting indices of the substrings of A which matches any of the anagrams of B.

### Constraints

1 <= length of the string A,B <= 100000

length of string A > length of string B

'a' < = A[i] ,B[i] < ='z'

### For Example
```
Input 1:
    A = "BACDGABCDA"
    B = "ABCD"
Output 1:
    [0, 5, 6]

Input 2:
    A = "AAABABAA"
    B = "AABA"
Output 2:
    [0, 1, 4]
```

## Solution
### Editorial
```cpp
vector<int> Solution::solve(string A, string B) {
    vector<int>v;
    sort(B.begin(),B.end());
    int lenb=B.length();
    int lena=A.length();
    for(int i=0;i<=lena-lenb;i++)
    {
        string s=A.substr(i,lenb);
        sort(s.begin(),s.end());
        if(s.compare(B)==0)
        v.push_back(i);
    }
    return v;
}
```

### Fastest
```cpp
vector<int> Solution::solve(string A, string B) {
    vector<int>temp1(26,0),temp2(26,0);
    for(int i=0;i<B.length();i++)
        temp2[B[i]-'a']++;
    vector<int>sol;
    for(int i=0;i<B.length()-1;i++)
        temp1[A[i]-'a']++;
    for(int i=0;i<=A.length()-B.length();i++)
    {
        temp1[A[B.length()+i-1]-'a']++;
        if(temp1==temp2)
        sol.push_back(i);
        temp1[A[i]-'a']--;
    }
    return sol;
}
```

### Lightweight
```cpp
void display(vector<int> a){
    // cout<<k<<endl;
    for(int i=0;i<a.size();i++)
    cout<<a[i]<<' ';
    cout<<endl;
}
bool same(vector<int> a, vector<int> b){
    int i=0;
    while(i<26){
        if(a[i]!=b[i])
        return false;
        ++i;
    }
    return true;
}
vector<int> Solution::solve(string A, string B) {
    int sum=0;
    vector<int> count(26,0),counta(26,0),out;
    for(int j=0;j<B.length();++j){
        count[B[j]-97]++;
        sum++;
    }
        int l=sum;
    for(int i=0;i<A.length();++i){
        // display(count);
        // display(counta);
        if(i-l+1>0){
            counta[A[i-l]-97]--;
        }
        counta[A[i]-97]++;
        if(same(counta,count)){
            out.emplace_back(i-l+1);
        }
        // cout<<endl;
    }
    return out;
}
```

### Mine
```cpp
vector<int> Solution::solve(string A, string B) {
    vector<int> res;
    int n = B.size();
    unordered_map<char, int> m1, m2;
    for (char c:B) m1[c]++;
    for (int i=0; i<A.size(); i++) {
        m2[A[i]]++;
        if (i>=n) m2[A[i-n]]--;
        if (m1==m2) res.push_back(i-n+1);
    }
    return res;
}
```
